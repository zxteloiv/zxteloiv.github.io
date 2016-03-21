---
layout: page
title: build python 2.7.11
math_support: mathjax
---


~~~ bash
wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tar.xz
tar xJf Python-2.7.11.tar.xz 
cd Python-2.7.11

./configure --prefix=/home/zx/local/python27 --with-ensurepip=install --enable-ipv6 --enable-profiling --enable-framework
~~~


