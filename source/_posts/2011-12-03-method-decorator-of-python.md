---
title: Method Decorator of Python
author: torrge
comments: true
layout: post
permalink: /2011/12/method-decorator-of-python/
categories:
  - Programming
tags:
  - Python
---
After half of day, I have gotten the main idea of Method Decorator of Python.

First,  Decorator with no additional arguments except the method to be transformed

<pre class="brush:py">@staticmethod
def methodName(args...):
    #do sth</pre>

is equivalent to

<pre class="brush:py">def methodName(args...):
    #do sth
methodName = staticmethod(methodName)</pre>

Then, Decorator with arguments except the method to be transformed

<pre class="brush:py">def decorator(**attrs):
    def wrapper(method):
        for k in attrs:
            setattr(method,k,attrs[k])
        return method
    return wrapper

@decorator(version='x.x.x', author='whatever')
def methodName(args...):
    #do sth</pre>

is equivalent to

<pre class="brush:py">def methodName(args...):
    #do sth
methodName = decorator(version='x.x.x', author='whatever')(methodName)</pre>

Finally, let&#8217;s write piece of code  kind of complex. This decorator adds some attributes to method transformed, and verifies if the argument of the method to be transformed is an int type instance

<pre class="brush:py">def decorator(**kwds):
    def wrapper(method):
        def verify(arg):
            assert isinstance(arg,int)
            return method(arg)
        for k in kwds:
            setattr(verify, k, kwds[k]):
        return verify
    return wraper

@decorator(version='x.x.x', author='whatever')
def methodName(arg):
    #do sth</pre>

is equivalent to

<pre class="brush:py">def methodName(arg):
    #do sth
methodName = decorator(version='x.x.x',author='whatever')(arg)</pre>

As expected, there is also Class Decorator for Python, similar with Method Decorator.

ref: <a href="http://www.python.org/dev/peps/pep-0318/" target="_blank">PEP 0318</a>