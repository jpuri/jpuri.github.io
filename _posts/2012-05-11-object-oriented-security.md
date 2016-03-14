---
layout: post
title:  "Object Oriented Security."
date:   2012-05-11 00:00
categories: General
permalink: archivers/object-oriented-security
---
For making out applications more secure we generally make use of specialized separate code for security, some api, etc. But recently I came across this term Object Oriented Security. It is about using some good Object Oriented Design Patterns to make your software inherently more secure. I feel it is worth giving a look, when I look at the Cross Site Scripting security issue I see that credit to it goes to violations of some of the very basic OO Principles by JavaScript.

Object oriented software enjoys many of the implicit security advantages. Having a reference to an object implies the authentication to use it. There cannot be a third object interfering when an object calls other. Security in Object Oriented applications can be effected by controlling access to objects. Central is thus the Principle of Least Privilege or Principle of Least Authority; give an Object all that it needs and nothing else.

Some of the Design Patterns that can really help are: 1. Never pass values to a function that it will not need. Give the minimum set of what is required to get the work done. For instance I have a function for finding area:

`int area(int x, int y){ return x * y; }` I can call this method as `area(x,y)` or `area(x,y,z)`

It will work in both cases (not for strongly typed languages) but the second way should be always avoided.

    1. Pass to the function the result and not the machinery. Take an example of an authentication method which matches the password entered by user with the one in the DB.
Bad way to design this method is:

`verifyPassword(userId, enteredPwd, userList){ // using userId as key obtain password from userlist // verifying the passwords for equality. }`

And the good way is:

`verifyPassword(enteredPwd, dbPwd){ // verifying the passwords for equality. }`

This was a case of method which was only reading the values. In case the method updates value,

Bad way could be something like:

updatePassword(userId, newPwd, tableRecordList){ // update table record and save it } Function call: updatePassword(‘U10’, newPwd, tableRecordList)

A better way would be to rather pass your update function or even better a closure to the function:

updatePassword(userId, newPwd, updateFunction){ //updateFunc.call(userId, newPwd) } createUpdateFunction(){ userList = UserTable.list() return updateFunction(userId, newPwd){ // update user record for userId and save it
} }` Function call: updatePassword(‘U10’, newPwd, createUpdateFunction())

Thus we have saved ourselves from passing list of DB records which function could have manipulated.
A good design practice is also to restrict usage of object by wrapping it in narrower interface. For instance I have in my application a type file which allows read/ write of files. For read only operations I can create a interface which exposes only methods to read file and not write.
`class file{ String readFile(){ ----- } void writeFile(String str){ ----- } }

            interface fileReader{
        void readFile(String str)
}`

Using revocable forwarders, a wrapper can be created over another powerful object, the wrapper will forward the call to the object and also provide a revoke method which will break link with the underling object.
Class RevokableFileWrapper{ File file; void forward(){ file.call(); } void revoke(){ file = null; } }

As our required intent is done we can then call revoke method on the object to break link to underlying object so that the object can than not be manipulated anywhere else.

Security thus becomes an integral part of well designed software. Security is thus not left to the mercy of some complicated and separated third party api. But is very much there in entire application. Thus a change is needed in the coding style and like maintainability, performance, etc security is very much there in background of the application but users do not notice it and are not annoyed by it.
