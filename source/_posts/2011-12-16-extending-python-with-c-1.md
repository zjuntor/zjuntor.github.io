---
title: Extending Python with C (1)
author: torrge
comments: true
layout: post
permalink: /2011/12/extending-python-with-c-1/
categories:
  - Programming
tags:
  - C
  - Python
---
Recently, I read about how to extend Python(CPython) with C.  Beyond my expectation, the macros, variety of predefined functions and types in source files are so difficult to read that  I have made such a tiny progress.

There are three main aspects of extending Python, build-in functions(methods), new types and embedding Python in other languages(C).  I just have picked up the first, extending build-in functions (methods).

Support that there is a C function to calculate factorial of a natural number.

<pre class="brush:applescript">int fact(int n){
    if(n &lt;=1)
        return 1;
    else
    return n * fact(n-1);
}</pre>

To use this function in Python, we need encapsulate it, and add it to a Python module.

First, using Python C API to encapsulate function &#8220;fact&#8221;

<pre class="brush:cpp">PyObject* wrapper(PyObject* self,PyObject* args){
    int n,res;
    if(!PyArg_ParseTuple(args,"i",&n))
        return NULL;
    res = fact(n);
    return Py_BuildValue("i",res);
}</pre>

Then initial a method table, add function &#8220;wrapper&#8221; to it

<pre class="brush:applescript">static PyMethodDef DemoMethods[] = {
    {"fact",wrapper,METH_VARARGS,"Calculate n!"},
    {NULL,NULL,0,NULL}
};</pre>

At last, initial a new module with method above

<pre class="brush:cpp">void initdemo(){
    (void) Py_InitModule("demo",DemoMethods);
}</pre>

Now we can write a setup.py script to install and use it

<pre class="brush:py">from distutils.core import setup,Extension
    setup(
    name = 'demo',
    version = '0.0.1',
    ext_modules = [Extension('demo',['demo.c'])],
)</pre>

Install (recommended in virtualenv) and use it

<pre class="brush:shell">python setup.py install
&gt;&gt;&gt; import demo
&gt;&gt;&gt; demo.fact(10)
3628800
&gt;&gt;&gt;</pre>

This demo works well on my Fedora 16 Linux. For a win32 user, you can use a MSVC compiler or use GNU gcc compiler with setup.cfg file in the same directory of setup.py, and specify the compiler as below

<pre class="brush:shell">[build]
compiler=mingw32</pre>

ref: <a href="http://docs.python.org/extending/index.html" target="_blank">Extending and Embedding the Python Interpreter</a>