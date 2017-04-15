---
title: Postgres on Docker for a development environment
author: Brad McCormack
type: post
date: 2015-06-02T10:02:30+00:00
url: /operating-systems/postgres-on-docker-for-a-development-environment/
spacious_page_layout:
  - default_layout
dsq_thread_id:
  - 3822250127
categories:
  - Database
  - Development
  - Hosting
  - Information Technology
  - Linux
  - Networking
  - Operating Systems
  - Virtualization

---
* This is a work-in-progress post that I will finish. I&#8217;m putting it up to encourage me to finish it sooner 😛

### Terms

  * Host &#8211; My laptop. The computer running the docker host
  * Docker Host &#8211; a vm running under the host that will in turn run all of the docker containers.
  * Docker Container &#8211; a container running under the Docker Host &#8211; each Postgres instance or Redis instance. 
  * Postgres shell or a GUI application such as pgadmin(which I'll be using).

### Requirements

  * Hypervisor set up &#8211; KVM / VMWare/ Virtual box configured.
  * Ubuntu 14.04 or 14.10.
  * Up to date docker 
  - I'm currently working on a project that would benefit from having multiple Postgres nodes running for various benefits such as replication, fail over and load balancing.
  
As part of the requirement of setting up this environment I will need to have at least two nodes running. One the master, the other the slave. I could use a few cheap VPS&#8217;s or even one VPS and install docker on it but I&#8217;m just using my laptop and a kvm host running Ubuntu. I have more performance at my disposal without having to spend extra on a beefy enough VPS.

I&#8217;ll proceed now with my set-up
    
    
### Notes

* I use Arch Linux at home.
* You could also install a PHP web interface on the docker guests but I'll avoid that.

<pre>
<code class="language-bash">
sudo pacman -S pgadmin3
</code>
</pre>

### Setting up the Ubuntu docker host.

Install Ubuntu &#8211; Please see elsewhere for this

Install some pre-reqs and start them

<pre>
<code class="language-bash">
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get install ssh
sudo apt-get install htop
sudo service ssh start
sudo service ssh enable
# This is the official documentation. However, piping to a shell is not a good idea  
curl -sSL https://get.docker.com/ubuntu/ | sudo sh
source /etc/bash_completion.d/docker
</code>
</pre>

    
### Setting up docker

<pre>
<code class="language-bash">
sudo service docker start
sudo docker run -p 5432:5432 &#8211;name postgres-master &#8211;restart=always -e POSTGRES_PASSWORD=postgres -d postgres
</code>
Unable to find image &#8216;postgres&#8217; locally

Pulling repository postgres

60f3fc146ba7: Download complete
511136ea3c5a: Download complete
1aeada447715: Download complete
479215127fa7: Download complete
23d3cb178bf3: Download complete
91bf88457519: Download complete
d554656768a7: Download complete
de76b0ee8174: Download complete
cd77e618ce58: Download complete
979d210e7fe3: Download complete
3548242ce0ea: Download complete
23255fa2e8ba: Download complete
91f994ce62d4: Download complete
6a305e69ed0a: Download complete
029994d48265: Download complete
e0bdffa8d3d1: Download complete
066462b466c4: Download complete
d0a98b6e54ab: Download complete
96b828efe2e4: Download complete
1405cf275560: Download complete
50a67bb0d077: Download complete
128bd3d820ee: Download complete
40595a3e7e25384e391aaa465ec198e6dc3f6707a8eb827ae248c6d73a293502
</pre>

<pre>
<code class="language-bash">
sudo docker ps
</code>

CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
40595a3e7e25 postgres:latest &#8220;/docker-entrypoint. 24 minutes ago Up 24 minutes 5432/tcp postgres-master

<code class="language-bash">
sudo docker inspect postgres-master | grep IPAddress
</code>
&#8220;IPAddress&#8221;: &#8220;172.17.0.2&#8221;
<code class="language-bash">
telnet 172.17.0.2 5432
</code>

Trying 172.17.0.2&#8230;
Connected to 172.17.0.2.

Escape character is &#8216;^]&#8217;.
</pre>

From the above we can see that the docker container is running, Postgres is running and it has an ip of 172.17.02 and an exposed port of 5432 which I can telnet into just fine. I have also mapped 5432 on the host to 5432 in the guest so I will be able to access it on my host.

Now lets also start the slave

<pre>
<code class="language-bash">
sudo docker run -p 5433:5432 &#8211;name postgres-slave &#8211;restart=always -e POSTGRES_PASSWORD=postgres -d postgres
sudo docker ps aux
</code>
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
0eb635fd9da6 postgres:latest &#8220;/docker-entrypoint. 8 seconds ago Up 7 seconds 0.0.0.0:5433->5432/tcp postgres-slave
d405017c5768 postgres:latest &#8220;/docker-entrypoint. 4 minutes ago Up 4 minutes 0.0.0.0:5432->5432/tcp postgres-master
</pre>
    
WOW- Docker is insanely cool. It took 2 seconds to start a new instance of Linux with Postgres running ready to go. I can access one via 5433 and one via 5432 on the host.

Lets point pgadmin to these Postgres instances. Below is a screenshot showing this.

[<img src="https://www.nerdoncoffee.com/wp-content/uploads/2015/01/pgAdmin-ServersView.png" alt="pgAdmin-ServersView" width="1920" height="766" class="aligncenter size-full wp-image-721" />][1]

#### Setting up Master &#8211; Slave streaming replication 

Great guides have already been written which are referenced in the References below. However I&#8217;ll note anything specific to my set-up here.

### Make sure that the containers can access each other

Docker linked containers

First things first we need to jump into each container and set up a few things.

<pre>
<code class="language-bash">
sudo docker inspect &#8211;format &#8216;{{ .NetworkSettings.IPAddress }}&#8217; postgres-master
</code>
172.17.0.4
<code class="language-bash">
sudo docker inspect &#8211;format &#8216;{{ .NetworkSettings.IPAddress }}&#8217; postgres-slave
</code>
172.17.0.5
</pre>
    
So lets jump onto postgres-master and do everything (update stuff while we are there and commit it on the host)
<pre>
<code class="language-bash">
sudo docker exec -i -t postgres-master bash
apt-get update && apt-get upgrade
apt-get install vim
</code>
</pre>
    
Now lets commit those updates
<pre>
<code class="language-bash">
sudo docker commit postgres-master postgres/brad
sudo docker images
</code>
REPOSITORY TAG IMAGE ID CREATED VIRTUAL SIZE
postgres/brad latest 1be78fffd1f4 5 seconds ago 263.5 MB
</pre>
    
Now let's configure replication on the master

<pre>
<code class="language-bash">
vim \`find / -name postgresql.conf\`
\# Make sure values are set as per https://gist.github.com/greinacker/4968619#file-master_postgresql-conf
vim \`find / -name pg_hba.conf\`
\# Make sure values are set as per https://gist.github.com/greinacker/4968619#file-master\_pg\_hba-conf but substitute the ip with that from inspect postgres-slave above e.g
\# hostssl replication replicator 172.17.05 md5
su postgres
</code>
</pre>
    
Now let's start the slave again and link it to the master

<pre>
<code class="language-bash">
sudo docker stop docker-slave
sudo docker rm docker-slave
# Note it uses the new image with os updates and vim ready (postgres/brad)
sudo docker run -p 5433:5432 &#8211;name postgres-slave &#8211;restart=always &#8211;link postgres-master:postgres-master -e POSTGRES_PASSWORD=postgres -d postgres/brad
sudo docker ps
</code>
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
6dedb3e39f80 postgres/brad:latest &#8220;/docker-entrypoint. About a minute ago Up About a minute 0.0.0.0:5433->5432/tcp postgres-slave
fcea08d998b9 postgres/brad:latest &#8220;/docker-entrypoint. 3 minutes ago Up 3 minutes 0.0.0.0:5432->5432/tcp postgres-master 
</pre>

Now lets confirm its all working.

<pre>
<code class="language-bash">
sudo docker exec -i -t postgres-slave bash
cat /etc/hosts
172.17.0.8 6dedb3e39f80
127.0.0.1 localhost
::1 localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
</code>
</pre>

You can see that postgres-master alias is already added by docker lets see if we can ping it
<pre>
<code class="language-bash">
172.17.0.6 postgres-master
root@6dedb3e39f80:/# ping postgres-master
</code>
PING postgres-master (172.17.0.6): 48 data bytes
56 bytes from 172.17.0.6: icmp_seq=0 ttl=64 time=0.182 ms
56 bytes from 172.17.0.6: icmp_seq=1 ttl=64 time=0.168 ms
</pre>
YAY
  
### References

  * http://www.liquidweb.com/kb/how-to-install-docker-on-ubuntu-14-04-lts/
  * https://docs.docker.com/installation/ubuntulinux/
  * https://registry.hub.docker.com/_/postgres/
  * http://www.rassoc.com/gregr/weblog/2013/02/16/zero-to-postgresql-streaming-replication-in-10-mins/
  * https://www.google.com.au/webhp?sourceid=chrome-instant&ion=1&espv=2&es_th=1&ie=UTF-8#q=postgres%20master%20slave
  * https://docs.docker.com/articles/host_integration/
  * https://docs.docker.com/userguide/dockerlinks/

[1]: https://www.nerdoncoffee.com/wp-content/uploads/2015/01/pgAdmin-ServersView.png