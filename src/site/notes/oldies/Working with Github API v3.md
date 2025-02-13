---
{"dg-publish":true,"permalink":"/oldies/working-with-github-api-v3/","dgShowInlineTitle":true,"dgShowToc":true,"dgLinkPreview":true,"created":"02/08/2012"}
---



I was pleasantly surprised when I needed to use the [Github
API](http://develop.github.com/p/general.html) for a project. The API is
dead simple to use, retrive and iterate data for almost any repo based
stat you can imagine. This is just a quick and dirty GET example that
makes a list of all the repos I follow on Github.

aside - The result below is a real-time demo using the
[jinX](http://www.jqueryin.com/projects/jinx-javascript-includer-wordpress-plugin/)
wp plugin to inject javascript into posts

```javascript
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

```
