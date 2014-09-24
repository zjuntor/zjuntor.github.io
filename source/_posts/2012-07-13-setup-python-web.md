---
title: Setup Python Web
author: torrge
comments: true
layout: post
permalink: /2012/07/setup-python-web/
categories:
  - Programming
tags:
  - bottle
  - Nginx
  - Python
  - uwsgi
  - Web
  - WSGI
---
This weekend, I tried to setup Python Web environment. At beginning, I tried Apache http server & mod_python, but shamefully failed.  Then, I turned to Nginx & uwsgi.

Nginx, as its official claimed, is a high performance http server & reverse proxy. As a reverse proxy, in my perception, it just responses the request and then distributes it to the according server which is the one really processing the request.  In the Nginx & uwsgi couple, the real server to process request is uwsgi, which implements its own uwsgi protocol & wsgi protocal.

Using this architecture, there several advantages, for example, we can use Nginx as a load balancer, in spite of that I don&#8217;t really understand this detailly.

install nginx:

<pre class="brush:shell">#in extracted folder, may need su privilege
./configure --prefix=where/to/install/ngnix
make & make install</pre>

install uwsgi:

<pre class="brush:shell">#using virtualenv
source path/to/yourvirtualenv/bin/activate
easy_install uwsgi</pre>

configure nginx:

<pre class="brush:plain">#in nginxhome/conf/nginx.conf
server{
		listen 8081;
		server_name localhost;
		location /{
			include uwsgi_params;
			uwsgi_pass localhost:3031;
			root  path/to/demo/;		
		}	
	}</pre>

write wsgi script(in folder path/to/demo/):

<pre class="brush:applescript">from bottle import route, run, default_app  
@route('/demo')  
def index():  
	return "it works."
if __name__ == '__main__':
	run(host='localhost', port=3032)
else:
	application = default_app()</pre>

now, start nginx & uwsgi:

<pre class="brush:shell">nginxhome/sbin/nginx
#in virtualenv
uwsgi --socket localhost:3031 -H virtualenvname --wsgi-file path/to/demo.py</pre>

something to be marked:

1, during install, using ldd to make out the dependencies;

2, there more details to configure of nginx & uwsgi if needed.

3, in many examples, to start uwsgi, &#8216;&#8211;http&#8217; option is used, but that may failed except using &#8216;&#8211;socket&#8217; instead.