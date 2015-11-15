---
layout: post
section-type: post
title: Flickr API tutorial
category: TUTORIALS
tags: [ 'tutorials' ]
---
I recently made a photo porfolio generator using Flickr API and Angular. The <a href = "https://www.flickr.com/services/api/">documentation of Flickr API </a>is very well written and easy to use. 

here is an example using the <a href="https://www.flickr.com/services/api/flickr.photosets.getList.html">"flickr.photosets.getList</a> method: 
<pre><code>

  var findPhotoSetByUser = "https://api.flickr.com/services/rest/" + // connect to the Flickr API
    "?method=flickr.photosets.getList" + // returns all the photosets belong to the user
    "&api_key=fd661e881fcea083ba46a27d0065ac8d" + // your API key
    "&user_id=" + userId + // The user ID.
    "&privacy_filter=1" + // 1 signifies all public photos.
    "&per_page=100" + // limiting it to 100 photos.
    "&format=json&nojsoncallback=1"; // return in JSON format. 
</code></pre>

Instead of writing this yourself, you could also <a href = "https://www.flickr.com/services/api/explore/flickr.photosets.getList">"API Explorer : flickr.photosets.getList </a>to generate these code for you. All the API explorer links for each method can be found at the bottom of each method's page.


<a href ='http://development.flickr-angular.divshot.io/'>Finished product</a>

