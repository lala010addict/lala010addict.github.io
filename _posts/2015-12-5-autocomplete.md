---
layout: post
section-type: post
title: jQuery UI autocomplete with AngularJS
category: TUTORIALS
tags: [ 'tutorials' ]
---

I was working on a TV show recommendation app that allows users to visualize the show ratings using D3.js. One of the features I worked on was the autocomplete feature for show names. The autocomplete feature searches in a movie database using <a href ="https://www.themoviedb.org/">a movie database API</a> and suggests show names as a user types in the name of the show they are trying to search for. 

![](/blogimgs/autocomplete/autocomplete.jpg)

I used <a href="https://jqueryui.com/autocomplete/">jQueryUI Autocomplete</a> for the autocomplete feature. A sample jQuery code should look like something like this:

<pre><code>
$(function(){
    $("#SEARCHFIELD").focus(); //Focus on search field
    $("#SEARCHFIELD").autocomplete({
        minLength: 2,
        delay:5,
        source: "MOVIEDB_API_URL",
        focus: function( event, ui ) {
            $(this).val( ui.item.value );
            return false;
        },
        select: function( event, ui ) {
            $(this).val( ui.item.value );
            return false;
        }
    }).data("uiAutocomplete")._renderItem = function( ul, item ) {
        return $("<li></li>")
            .data( "item.autocomplete", item )
            .append( "<a>" + (item.img?"<img class='imdbImage' src='imdbImage.php?url=" + item.img + "' />":"") + "<span class='imdbTitle'>" + item.label + "</span>" + (item.cast?"<br /><span class='imdbCast'>" + item.cast + "</span>":"") + "<div class='clear'></div></a>" )
            .appendTo( ul );
    };
});
</code></pre>

Something like this would have worked if I wasn't using Angular, but jQuery and Angular don't always want to work together. I had to wrap this jQuery UI autocomplete into an angular directive for it to work:

<pre><code>
var ImgPath = "http://image.tmdb.org/t/p/";

angular.module('app.autocomplete', [])
  .directive('autocomplete', ['$http', function($http) {
    return function(scope, elem, attrs) {
      elem.autocomplete({
        minLength: 2,
        delay: 200,
        source: function(request, response) {
          $http({
            method: "JSONP",
            url: "https://api.themoviedb.org/3/search/tv?api_key=YOUR_API_KEY&query=" + request.term + '&callback=JSON_CALLBACK'
          }).success(function(data) {
            response(data.results);
          });
        },
        focus: function(event, ui) {
          elem.val(ui.item.name);
          //  console.log(ui.item.name)
          return false;
        },
        select: function(event, ui) {
          scope.query = ui.item.name;
          console.log(scope.query)
          scope.submit(ui.item.name)
          return false;
        },
        change: function(event, ui) {
          if (ui.item === null) {
            scope.queryId.selected = null;
          }
        }
      }).data("uiAutocomplete")._renderItem = function(ul, item) {
        if (item.poster_path) {
          var inner_html = "<a><img width='45' height='68' src=" + ImgPath + "w92" + item.poster_path + "> <strong>" + item.name + "</strong>  " + item.first_air_date + " </a>";
          return $("<li></li>")
            .data("ui-autocomplete-item", item)
            .append(inner_html)
            .appendTo(ul);
        } else {
          var inner_html = "<a> <strong>" + item.name + "</strong>  " + item.first_air_date + " </a>";
          return $("<li></li>")
            .data("ui-autocomplete-item", item)
            .append(inner_html)
            .appendTo(ul);
        }
      };
    }
  }]);
</code></pre>

There are couple more things you also need to look out for:

    1. Make sure the order you load Angular and jQuery is correct. You need to load jQuery before Angular on your index.html, otherwise, in this case it would give you an error such as 'elem.autocomplete is not a function' because it thinks .autocomplete is a part of angular.
    
    2. Make sure you are requesting JSONP instead of jus JSON because JSON would give you an 'Method JSON is not allowed by Access-Control-Allow-Methods in preflight response.' error. And make sure you are adding '&callback=JSON_CALLBACK' when you are using JSONP method :)


















