---
layout: post
title:  "Write a C extension for Python"
date:   2015-04-07 22:09:00
tags: python c extension
categories: note
---

This is a memo for writing C extensions for python.

Sometimes we need to use python but have some tools written in C/C++ at hand already.   
In my recent projects, I want to convert coordinates among several systems like wgs84, gcj02 and bd09ll.   
I write my service in python but we only have conversion function in C.  

I found two good reference:

1. A step by step tutorial here. http://dan.iel.fm/posts/python-c-extensions/
2. The python official document on writing extension. https://docs.python.org/2/extending/extending.html

My final work is published on [github](https://github.com/zxteloiv/pycoordtrans).

Here are a few things to note:
* Python2 and Python3 are different
* In some linux distributions like CentOS 6, python-devel package is needed for the included `Python.h` file, which can be installed by yum.
* Python function supports naming parameters, which needs some modification on the call of `PyArg_ParseTuple` function
