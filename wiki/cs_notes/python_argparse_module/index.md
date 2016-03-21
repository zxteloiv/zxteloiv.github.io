---
layout: page
title: python argparse module
math_support: mathjax
---


### parser

parser is a processor for all the arguments

description:
~~~python
import argparse

parser = argparse.ArgumentParser(description="program description")
~~~

### add argument

~~~python
parser.add_argument('foo', # name, alternatives are "--foo", "-f")
    
parser.add_argument('--foo', type=int)
parser.add_argument('--foo', type=file)
parser.add_argument('--width', default=10.5, type=int)
~~~

### reference

https://docs.python.org/2/library/argparse.html


