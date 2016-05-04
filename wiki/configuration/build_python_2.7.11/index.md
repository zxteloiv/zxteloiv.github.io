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

官方源>软件官方ppa>第三方ppa>软件官方预编译>make deb>>>自己编译

~~~ bash
sudo add-apt-repository fkrull/deadsnakes-python2.7
sudo apt update
sudo apt install python2.7
~~~


