---
layout: page
title: python_profiling_tools
math_support: mathjax
---


# Python profiling tools

 Sat 08 June 2013

This is a small addition to the [previous article about the debuggers](https://blog.ionelmc.ro/2013/06/05/python-debugging-tools/).

The stdlib has 3 backends ([cProfile and profile](http://docs.python.org/2/library/profile.html#module-cProfile) , [hotshot](http://docs.python.org/2/library/hotshot.html)) and countless 3rd party visualization tools, converters and whatnot. Where there are many tools there's plenty of bad advice ...

## If you don't know what are you looking for, just use a visualization tool!

There's so much advice to just roll out your own visualization tool, be it text using the [Stats](http://docs.python.org/2/library/profile.html#the-stats-class) from stdlib or graphical using libraries like [pycallgraph](http://pycallgraph.slowchop.com/) or [gprof2dot](http://code.google.com/p/jrfonseca/wiki/Gprof2Dot) the first reaction would be to actually start writing code to generate reports.

But this is a bad idea, you need to change that code to skew or drill down in the report and generally everything will be confusing if you're not looking for something specific. You will probably overlook that thing that is killing your performance.

## RunSnakeRun

You can install it with pip install RunSnakeRun or apt-get install runsnakerun or from [sources](http://www.vrplumber.com/programming/runsnakerun/).

RunSnakeRun is a well rounded tool, easy to integrate - you can just use it with the stdlib cProfile/profile, just specify the filename argument to [profile.run](http://docs.python.org/2/library/profile.html#profile.run), eg:

```
import cProfile
cProfile.run("main()", filename="my.profile")

```

After that, run this in the shell:

```
runsnake my.profile

```

It looks like this:

![](https://blog.ionelmc.ro/2013/06/08/python-profiling-tools/runsnakerun.png)

This is acceptable and will work well if you don't have freakishly huge profiles, eg: you just have 1 function than takes too much time.

There's a *memory profiling mode* in RunSnakeRun (requires [Meliae](https://launchpad.net/meliae)). To my shame I haven't tried it but it looks very useful if you want to visualize memory usage. It looks like [this](http://www.vrplumber.com/programming/runsnakerun/meliae-sample.png).

## KCachegrind

You can install it with with apt-get install kcachegrind or from [sources](http://kcachegrind.sourceforge.net/html/Download.html). For windows you can use [QCacheGrind](http://sourceforge.net/projects/qcachegrindwin/).

I really like this tool! It shows you call tree graphs, sortable call tables, call/callee maps, sourcecode, and you can filter everything. It is language agnostic - you probably know this tool if you come from a C/C++ background.

I like this over [RunSnakeRun](https://blog.ionelmc.ro/2013/06/08/python-profiling-tools/#runsnakerun) cause it's a lot more powerful:

- on the call tree graphs: you can sort, change layout/rendering in many ways or export to dot/png - [RunSnakeRun](https://blog.ionelmc.ro/2013/06/08/python-profiling-tools/#runsnakerun) doesn't even show a call tree graph.
- you can see sourcecode
- you got callee maps
- it could be less pain to install it (no wxPython dependency)

[KCachegrind](https://blog.ionelmc.ro/2013/06/08/python-profiling-tools/#kcachegrind) is worth using using over [RunSnakeRun](https://blog.ionelmc.ro/2013/06/08/python-profiling-tools/#runsnakerun) in large projects where it's not obvious what needs attention or you generally have very many function involved.

### Tools to generate KCachegrind profiles

I guess this is the only downside, you need to export in this specific format. But it is well supported:

- [pyprof2calltree](https://bitbucket.org/ogrisel/pyprof2calltree)
- django-extensions's [runprofileserver](https://github.com/django-extensions/django-extensions/blob/master/django_extensions/management/commands/runprofileserver.py)
- this [decorator](https://translate.svn.sourceforge.net/svnroot/translate/src/trunk/virtaal/devsupport/profiling.py)
- [kcachegrind-converters](http://packages.debian.org/en/stable/kcachegrind-converters)
- [repoze.profile](http://docs.repoze.org/profile/) - just set the cachegrind_filename
- [yappi](https://pypi.python.org/pypi/yappi/)

I'm using [this function decorator](https://gist.github.com/ionelmc/5735205) that just litters your /tmp with profile dumps.

Obligatory screenshot:

![](https://blog.ionelmc.ro/2013/06/08/python-profiling-tools/kcachegrind.png)

This entry was tagged as [python](https://blog.ionelmc.ro/tag/python/)[profiling](https://blog.ionelmc.ro/tag/profiling/)


