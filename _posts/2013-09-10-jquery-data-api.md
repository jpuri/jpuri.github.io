---
layout: post
title:  "jQuery Data API"
date:   2013-09-10 09:00:13
categories: general
permalink: archivers/jquery-data-api
---
How many times has it happened with you that on your view page you need to associate some data values with a UI component like Dropdown or Checkbox, a possible solution in such cases is to keep data in hidden variables in the HTML or in variable in JavaScript.

Well there was a similar scenario in our project, our project is Spring/ Hibernate project where we are using JSP for views and jQuery for writing java script. A couple of pages heavily used Java Script and we were required to maintain data on the page, we came across jQuery Data API for storing data.

jQuery Data API allows you to associate any data value with a specified element on  the UI and then later retrieve it later.

For adding data it simply: `jQuery.data( element, key, value )` And then to fetch it back: `jQuery.data(element, key)`

For example these are couple of line from our js file where we associate tape data with radio button: `$.data(radioObject, 'tapeData', tape);`
and later fetch the data when we need: `setTapeInfo($.data(radioObject, 'tapeData'));`

Here `_radioObject` is jQuery wrapper object over the UI element.

jQuery data api also ensures efficient memory management provided by jQuery. For Details refer: [jQuery Data](http://api.jquery.com/jQuery.data/)
