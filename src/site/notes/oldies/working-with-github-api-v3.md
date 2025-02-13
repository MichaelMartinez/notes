---
{"dg-publish":true,"permalink":"/oldies/working-with-github-api-v3/","created":"02/08/2012"}
---


Title: Working with Github Api v3
Date: 2012-02-08 13:56
Author: Michael
Category: JSON 
Tags: API
Slug: working-with-github-api-v3
Status: published

I was pleasantly surprised when I needed to use the [Github
API](http://develop.github.com/p/general.html) for a project. The API is
dead simple to use, retrive and iterate data for almost any repo based
stat you can imagine. This is just a quick and dirty GET example that
makes a list of all the repos I follow on Github.

aside - The result below is a real-time demo using the
[jinX](http://www.jqueryin.com/projects/jinx-javascript-includer-wordpress-plugin/)
wp plugin to inject javascript into posts

    :::javascript
    $( document ).ready( function () {

      var html = "";
      $.ajax( {
        url : "https://api.github.com/users/MichaelMartinez/watched",
        dataType : "jsonp",
        success : function ( returndata ) {
          $.each( returndata.data, function ( i, item ) {
            html += '' +
              '' + this.name + '' +
              '' +
              '' + 'Description: ' + this.description + '' +
              '' + 'Language: ' + this.language + '' +
              '' + 'Updated: ' + this.updated_at + '' +
              '' + 'Owner: ' + this.owner.login + '' +
              '' +
              '';
          } );
          $( '#result' ).append( html );
        }
      } );
    } );
