---
title: Something About Learning JavaScript
author: torrge
comments: true
layout: post
permalink: /2012/08/something-about-learning-javascript/
categories:
  - Programming
  - Study
tags:
  - JavaScript
  - Study
---
Recently, I learned JavaScript forward, using this <a href="http://book.douban.com/subject/3182419/" target="_blank">book</a>, along with the <a href="https://developer.mozilla.org/en-US/docs/JavaScript/Guide" target="_blank">JavaScript Guide</a> from Mozilla Developer Network. Here are some note about the key point to me.

`this` keyword

There is a statement in the <a href="https://developer.mozilla.org/en-US/docs/JavaScript/Guide/Expressions_and_Operators#this" target="_blank">Guide</a>:

> Use the `this` keyword to refer to the current object. In general, `this` refers to the calling object in a method.

The key point is the second sentence, which clearly and exactly explains that `this` keyword refers the calling object. This is enough, and the first sentence, let it go the hell.

Closure

After a function has returned, the local environment (local variables etc.) can not be collected immediately by GC, when there are some external reference to the local environment. This can be made by assigning a nested function which uses the local variables to an external variable in the function, or returning the nested function and assigning it to external variables. for example:

<pre class="brush:js">//closure#1
var getter,setter;
(function(){
    var value=0;
    getter = function(){return value;};
    setter = function(v){value = v;};
})();
//closure#2
function iterator(x){
    var i = 0;
    return function(){return x[i++];};
}</pre>

Constructor

A constructor is an function object just looks like normal function objects which is use to create new objects. The constructor property of an object is the constructor using which the object made(it&#8217;s a convenience and by default). When a constructor is called using `new` operator, there something happened behind the scene:

*   JavaScript engine creates a generic object and assignes it to `this` , and then passes it to the constructor.
*   assigning contructor.prototype to the internal property of object `this` refers, in Firefox the property is &#8216;\_\_proto\_\_&#8217;, which is used for making up the prototype chain. Then assign the constructor.prototype.constructor object to constructor property of object `this` refers.
*   finally, make up the rest of the object with the code in the constructor and then return the object `this` refers to implicitly.

Prototype

JavaScript is object-based, object-oriented language. it use prototype to implement the inheritance. For example:

<pre class="brush:applescript">var Shape = function(){ this.info = 'Shape'; }
var Circle = function(diameter){ this.diameter = diameter; }
Circle.prototype = new Shape();
Circle.prototype.constructor = Circle;</pre>

When try to access a property of an object, the JavaScript engine searches the object first, if not found, then up along the prototype chain to the end until the property has been found, if still not found, return undefined.