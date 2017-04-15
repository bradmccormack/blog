---
title: Add an external java lib to an Android project using Eclipse
author: Brad McCormack
type: post
date: 2012-05-24T22:29:36+00:00
url: /development/add-an-external-java-lib-to-an-android-project-using-eclipse/
dsq_thread_id:
  - 3523369396
categories:
  - Development

---
I&#8217;ve recently started to work on an Open Source Android game and as part of this process I knew I&#8217;d have to rely on third party libraries. </br>

One such library is this Box2D physics library that will do all the complex physics computations on my behalf.

I searched around and found a nice  [I&#8217;ve recently started to work on an Open Source Android game and as part of this process I knew I&#8217;d have to rely on third party libraries. </br>

One such library is this Box2D physics library that will do all the complex physics computations on my behalf.

I searched around and found a nice][1] .

My suggested steps to adding the library to your eclipse project are </br>

  1. Add a folder to your project by right clicking on your project and selecting New -> Folder. I suggest naming it libs 
  2. Check out the source for the lib
                 
     `I&#8217;ve recently started to work on an Open Source Android game and as part of this process I knew I&#8217;d have to rely on third party libraries. </br>

One such library is this Box2D physics library that will do all the complex physics computations on my behalf.

I searched around and found a nice  [I&#8217;ve recently started to work on an Open Source Android game and as part of this process I knew I&#8217;d have to rely on third party libraries. </br>

One such library is this Box2D physics library that will do all the complex physics computations on my behalf.

I searched around and found a nice][1] .

My suggested steps to adding the library to your eclipse project are </br>

  1. Add a folder to your project by right clicking on your project and selecting New -> Folder. I suggest naming it libs 
  2. Check out the source for the lib
                 
` 
  3. Add the library to your project by right clicking on your project and selecting Import -> File System and navigating to the lib directory and selecting it. You will now be able to select the jar. After this you will see the jbox2d.jar file listed under the libs folder 
  4. Now right click on the jbox2d.jar file and select Build Path -> Add to Build Path 

</br>
  
That&#8217;s it !
  
</br>

Now the harder part. Figuring out how to integrate the Box2D functionality with my objects and then render them to the screen using OpenGL ES 🙂

 [1]: http://code.google.com/p/androidbox2d/