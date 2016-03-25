---
layout: post
title:  "Javascript's Promise"
date:   2013-10-12 09:00:13
categories: general
permalink: archivers/javascripts-promise
---
This blog is about Javascript's Promise Object. Writing asynchronous functions with complex callbacks can at times result in lot of boilerplate and ugly code. Javascript Promise give the promise to simplify these.

Promises are outcome of an asynchronous method calls for example I/O calls or Remote calls. At any given point a promise can be in one of 3 states:

1. Pending, if asynchronous call is executing.
2. Resolved, if asynchronous call is successfully completed.
3. Rejected, if asynchronous call failed.

We can attach callbacks to Promise which will be executed as the promise is Rejected or Resolved, or we can attach any other arbitrary callback to the Promise object which will be executed as soon as invoked. Lets look into jQuery's implementation of Promise called Deferred. Suppose I have a function save which I call on my model objects and it saves the objects remotely. I can implement it using Deferred Objects as:

        ModelObject.save = function({key: value}) {
          var deferred = new $.Deferred();
          MyUtil.saveRemotely({key: value}, function() {
            success: function() {
              deferred.resolve();
            };
            failure: function() {
              deferred.reject();
            };
          });
        return deferred.promise(); }

Now as user calls function save() he will get as return value a promise object. The user can then register call backs on this promise object to be invoked as promise is resolved or rejected.

        var myPromiseObject = myModelObject.save({'id': 1});

        myPromiseObject.done(fuction() {
          console.log('Saved successfully');
        });

        myPromiseObject.fail(fuction(){
          console.log('Error while trying to save');
        });

        myPromiseObject.always(fuction(){
          console.log('Save method completed.');
        });

These call backs can even be chained. There is another call back then which takes 3 functions, 1st will be invoked in case of success, 2nd in case of failure and 3rd will be executed always.

        MyPromiseObject.then( fuction() {
            console.log('Saved successfully');
          }, fuction() {
            console.log('Error while trying to save');
          }, fuction() {
            console.log('Save method completed.');
          }
        }

Thus a complex pyramid of call backs like:

    function1(value, function(value1) {
      function2(value1, function(value1) {
        function3(value2, function(value2) {
          function4(value3, function(value3){
          });
        });
      });
    });

can be reduced to:

        function1(value).then(function2).then(function3).then(function4)

Promises are now being increasingly used in various JavaScript Api and Frameworks. The are great tools in next generation of JavaScript design patterns.
