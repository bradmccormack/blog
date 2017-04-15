---
title: 'Android Activity –  onNewIntent not being fired'
author: Brad McCormack
type: post
date: 2012-08-28T09:30:59+00:00
url: /development/android-activity-onnewintent-not-being-fired/
dsq_thread_id:
  - 3363531773
spacious_page_layout:
  - default_layout
categories:
  - Android
  - Development

---
For those of you out there who find their activity is not firing onNewIntent (in the context of creating a searchable activity) there are many things to check such as

  * The activity is being destroyed on a state change such as orientation
  * String literals in the searchable.xml
  * [Various others.][1] 

I spent a couple of hours going down the wrong track. A heads up might be to check where you  have placed the searchable meta-data in your Android application manifest.

By moving my searchable to the layout directory rather than xml (under res) and moving the meta-data to the activity level in the xml the onNewIntent started to fire and I could respond accordingly
  
Such as ..

[code language=&#8221;java&#8221;]
  
if (Intent.ACTION_SEARCH.equals(intent.getAction())) {
    
String query = intent.getStringExtra(SearchManager.QUERY);
  
}
  
[/code]

In summary make sure the manifest looks something like

[code language=&#8221;xml&#8221;]
  
<application>
  
&#8230;

android:theme=&#8221;@android:style/Theme.Holo.Light&#8221;>;
  
<activity>
  
android:name &#8230;
  
android:configChanges=&#8221;orientation|screenSize&#8221;
  
android:label=&#8221;@string/app_name&#8221;
  
android:launchMode=&#8221;singleTop&#8221;>
  


<meta-data android:name="android.app.searchable"
android:resource="@layout/searchable" />
;

  
[/code]

 [1]: http://www.google.com.au/search?q=google+onnewintent+not+being+fired&oq=google+onnewintent+not+being+fired&sugexp=chrome,mod=14&sourceid=chrome&ie=UTF-8