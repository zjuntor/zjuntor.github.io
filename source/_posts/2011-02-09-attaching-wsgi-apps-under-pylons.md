---
title: Attaching WSGI apps under Pylons
author: torrge
comments: true
layout: post
permalink: /2011/02/attaching-wsgi-apps-under-pylons/
categories:
  - Programming
tags:
  - Pylons
  - Python
  - WSGI
---
Pylons 对WSGI 标准(<a href="http://www.python.org/dev/peps/pep-0333/" target="_blank">PEP-0333</a>) 进行了扩展，提升了性能，同时Pylons 也支持 Normal WSGI  Apps。

1. 创建一个Pylons 项目

<pre class="brush:shell">paster create -t pylons wsgitest</pre>

2. 创建一个 controller

<pre class="brush:shell">cd wsgitest
paster controller wsgiapp</pre>

3. 修改controller(controllers/wsgiapp.py)

<pre class="brush:py">def WsgiappController(environ, start_response):
    start_response('200 OK', [('Content-type', 'text/plain')])
    return ["It works!"]</pre>

注意：一开始的时候我将Pylons 项目的名字直接命名为‘WSGI’ 试了几次都不能Work，从Traceback 来看报错在controllers/wsgiapp.py 的第三行有语法错误，显然事实并不如此。猜测 项目名WSGI 跟Pylons 内部有些地方可能冲突了。

参考：<a href="http://www.python.org/dev/peps/pep-0333/" target="_blank">PEP-0333</a>