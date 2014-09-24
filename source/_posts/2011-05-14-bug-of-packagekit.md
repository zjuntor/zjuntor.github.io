---
title: Bug of PackageKit
author: torrge
comments: true
layout: post
permalink: /2011/05/bug-of-packagekit/
categories:
  - Programming
tags:
  - Fedora
  - Linux
---
On doing recent update of my Fedora 15，I got an error message as follow:

> Error Value: &#8216;YumFilter&#8217; object has no attribute &#8216;pre_process&#8217;  
> &#8230;

Some guy has reported this bug to Redhat:

> Bug detail: <a href="https://bugzilla.redhat.com/show_bug.cgi?id=702501" target="_blank">Bug 702501</a>** **

Solution:

> sudo yum update PackageKit

After having the PackageKit update installed，update works well again.