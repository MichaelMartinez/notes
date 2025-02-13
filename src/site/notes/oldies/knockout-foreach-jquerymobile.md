---
dg-show-inline-title: false
dg-publish: true
dg-home: false
dg-show-toc: false
dg-created: 02/22/2012
dg-updated: 
dg-show-tags: false
dg-link-preview: false
---

Title: Using Knockoutjs's foreach on nested jQuery Mobile lists
Date: 2012-02-22 19:34
Author: Michael
Category: JavaScript 
Tags: jQuery Mobile, knockout
Slug: knockout-foreach-jquerymobile
Status: published

I just want to drop a quick note to anyone who may search google for a
problem I ran into.A nested jQuery mobile list can be populated with one
knockoutjs observable array. The only caveat I've seen thus far is that
you need to use the foreach data-binding twice. Once on the outer list
and again for the inner "list" or page.


