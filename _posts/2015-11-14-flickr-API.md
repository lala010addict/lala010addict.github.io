---
layout: post
section-type: post
title: Flickr API tutorial
category: TUTORIALS
tags: [ 'tutorials' ]
---
I recently made a photo porfolio generator using Flickr API and Angular. The documentation of Flickr API is very well written and easy to use. 

here is an example: 
<pre><code>

  var findPhotoSetByUser = "https://api.flickr.com/services/rest/" + // connect to the Flickr API
    "?method=flickr.photosets.getList" + // Get photo from a photoset
    "&api_key=fd661e881fcea083ba46a27d0065ac8d" + // your API key
    "&user_id=" + userId + // The set ID.
    "&privacy_filter=1" + // 1 signifies all public photos.
    "&per_page=100" + // limiting it to 100 photos.
    "&format=json&nojsoncallback=1"; // return in JSON format. 
</code></pre>

<a href ='http://development.flickr-angular.divshot.io/'>Finished product</a>

