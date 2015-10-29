---
layout: post
title: "Add SVG elements using D3.js and Spin multiple elements around their own center with GSAP"
image: false
video: false
comments: true
tags: [ 'tutorials' ]
---

I was making a game using D3.js to add SVG elements and I needed to make multiple SVG elements/shapes to rotate at their own center.

Before we get to the rotation part, let's create some SVG shapes using D3.js:

Creating a board to hold my SVG elements:
<pre><code>var board = d3.select('body')
.append('svg')
.attr('height', 300)
.attr('width', 400)
.style('background-color', 'black')
</code></pre>

Creating six ellipses:
<pre><code>var elli = board.selectAll('.elli')
.data(d3.range(6))                  // creating six
.enter()
.append('ellipse')                  //creating ellipses
.attr('rx', 5)                      //height
.attr('ry', 14)                     //width
.attr('cx', function() {            //place at random positions
return (Math.random() * 300);})
.attr('cy', function() {
return (5+Math.random() * 250);})
.attr('fill', 'deepPink');          //make them pink
</code></pre>

Now we should have something looks like this:
six pink ellipses placed that random positions.
![](/blogimgs/Screen-Shot-2015-10-18-at-4-23-53-PM.png)

Let's get them to rotate:

SVG has some very easy to use <a href ='https://developer.mozilla.org/en-US/docs/Web/SVG/Element/animateTransform'>animations</a> such as rotation, but however it rotates in in circles around a point that's at the top left corner of the SVG canvas if you don't specify the center to be the center of the SVG canvas. 

![](/blogimgs/a.jpg)

I found a helpful [article](https://css-tricks.com/guide-svg-animations-smil/) that teaches how to set the center of the rotation. 



<p data-height="268" data-theme-id="0" data-slug-hash="pjdOMa" data-default-tab="result" data-user="lala010addict" class='codepen'> <a href='http://codepen.io/lala010addict/pen/pjdOMa/'></a>  <a href='http://codepen.io/lala010addict'></a>  <a href='http://codepen.io'></a></p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

... but I did not get it to work on multiple SVG elements at the same time. After hours of research, I found GSAP: 
>It animates anything JavaScript can touch (CSS properties, canvas library objects, SVG, generic objects, whatever) and it solves lots of browser inconsistencies, all with blazing speed (up to 20x faster than jQuery).

to make them rotate:

<pre><code>function transformOriginRotation() {
tween.seek(0).kill();                  //reset
tween = TweenLite.to(ell, 1, {
rotation: 360,
transformOrigin: "50% 50%"           //set the center   
});
tweenCode.innerHTML = 'TweenLite.to(".elli", 1, {rotation:360, transformOrigin:"50% 50%"});'
};
setInterval(transformOriginRotation, 600);</code></pre>

taaaaadaaaaa... there we have it! Rotating ellipses!! Please click on 'edit on codepen' to see the animation. 


<p data-height="268" data-theme-id="0" data-slug-hash="epejav" data-default-tab="result" data-user="lala010addict" class='codepen'> <a href='http://codepen.io/lala010addict/pen/epejav/'></a>  <a href='http://codepen.io/lala010addict'></a> <a href='http://codepen.io'></a></p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>


























