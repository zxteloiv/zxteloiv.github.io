---
layout: page
title: python_debugging_tools
math_support: mathjax
---


# Python debugging tools

 Wed 05 June 2013

This is an overview of the tools and practices I've used for debugging or profiling purposes. This is not necessarily complete, there are so many tools so I'm listing only what I think is best or relevant. If you know better tools or have other preferences, please comment below.

## Logging

Yes, really. Can't stress enough how important it is to have adequate logging in your application. You should log important stuff. If your logging is good enough, you can figure out the problem just from the logs. Lots of time saved right there.

If you do ever litter your code with print statements stop now. Use logging.debug instead. You'll be able to reuse that later, disable it altogether and so on ...

## Tracing

Sometimes it's better to see what gets executed. You could run step-by-step using some IDE's debugger but you would need to know what you're looking for, otherwise the process will be very slow.

In the stdlib there's a [trace](http://docs.python.org/2/library/trace.html#cmdoption-trace-t) module which can print all the executed lines amongst other this (like making [coverage reports](http://docs.python.org/2/library/trace.html#cmdoption-trace-c))

```
python -mtrace --trace script.py

```

This will make lots of output (every line executed will be printed so you might want to pipe it through grep to only see the interesting modules). Eg:

```
python -mtrace --trace script.py | egrep '^(mod1.py|mod2.py)'

```

### Alternatives

Grepping for relevant output is not fun. Plus, the trace module doesn't show you any variables.

[Hunter](https://pypi.python.org/pypi/hunter) is a flexible alternative that allows filtering and even shows variables of your choosing. Just pip install hunter and run:

```
PYTHON_HUNTER="F(module='mod1'),F(module='mod2')" python script.py

```

Take a look at the [project page](https://github.com/ionelmc/python-hunter) for more examples.

―

If you're feeling adventurous then you could try [smiley](https://github.com/dhellmann/smiley) - it shows you the variables and you can use it to trace programs remotely.

Alternativelly, if you want very selective tracing you can use [aspectlib.debug.log](https://pypi.python.org/pypi/aspectlib) to make existing or 3rd party code emit traces.

## PDB

Very basic intro, everyone should know this by now:

```
import pdb
pdb.set_trace() # opens up pdb prompt

```

Or:

```
try:
    code
    that
    fails
except:
    import pdb
    pdb.pm() # or pdb.post_mortem()

```

Or (press c to start the script):

```
python -mpdb script.py

```

Once in the REPL do:

- c or continue
- q or quit
- l or list, shows source at the current frame
- w or where, shows the traceback
- d or down, goes down 1 frame on the traceback
- u or up, goes up 1 frame on the traceback
- <enter>, repeats last command
- ! <stuff>, evaluates <stuff> as python code on the current frame
- *everything else*, evaluates as python code *if it's not a PDB command*

## Better PDB

Drop in replacements for pdb:

- [ipdb](https://pypi.python.org/pypi/ipdb) (pip install ipdb) - like ipython (autocomplete, colors etc).
- [pudb](https://pypi.python.org/pypi/pudb) (pip install pudb) - curses based (gui-like), good at browsing sourcecode.
- [pdb++](https://pypi.python.org/pypi/pdbpp) (pip install pdbpp) - autocomplete, colors, extra commands etc.

## Remote PDB

sudo apt-get install winpdb

Instead of pdb.set_trace() do:

```
import rpdb2
rpdb2.start_embedded_debugger("secretpassword")

```

Now run winpdb and go to File > Attach with the password.

### Don't like Winpdb? Use PDB over TCP

Get [remote-pdb](https://pypi.python.org/pypi/remote-pdb) and then, to open a remote PDB on first available port, use:

```
from remote_pdb import set_trace
set_trace() # you'll see the port number in the logs

```

To use some specific host/port:

```
from remote_pdb import RemotePdb
RemotePdb(host='0.0.0.0', port=4444).set_trace()

```

To connect just run something like telnet 192.168.12.34 4444. Alternatively, run socat socat readline tcp:192.168.12.34:4444 to get line editing and history.

### Just a REPL

If you don't need a full blown debugger then just start a [IPython](http://ipython.org/install.html) with:

```
import IPython
IPython.embed()

```

If you don't have an attached terminal you can use [manhole](https://pypi.python.org/pypi/manhole).

## Standard Linux tools

I'm always surprised of how underused they are. You can figure out a wide range of problems with these: from performance problems (too many syscalls, memory allocations etc) to deadlocks, network issues, disk issues etc

The most useful is downright strace, just run sudo strace -p 12345 or strace -f command (-f means strace forked processes too) and you're set. Output is generally very large so you might want to redirect it to a file (just add &> somefile) for more analysis.

Then there's ltrace, it's just like strace but with library calls. Arguments are mostly the same.

And lsof for figuring out what the handler numbers you see in ltrace / strace are for. Eg: lsof -p 12345

### Better tracing

It's so easy to use and can do so many things - everyone should have htop installed!

```
sudo apt-get install htop
sudo htop

```

Now find the process you want, and press:

- s for system call trace (strace)
- L for library call trace (ltrace)
- l for lsof

### Monitoring

There's no replacement for good, continuous server monitoring but if you ever find yourself in that weird spot scrambling to find out why everything is slow and where are the resources going ... don't bother with [iotop](http://linux.die.net/man/1/iotop), [iftop](http://linux.die.net/man/8/iftop), [htop](http://linux.die.net/man/1/htop), [iostat](http://linux.die.net/man/1/iostat), [vmstat](http://linux.die.net/man/8/vmstat) etc just yet, start with [dstat](http://linux.die.net/man/1/dstat) instead! It can do most of the aforementioned tools do and maybe better!

It will show you data continuously, in a compact, color-coded fashion (unlike iostat, vmstat) and you can always see past data (unlike iftop, iotop, htop).

Just run this:

```
dstat --cpu --io --mem --net --load --fs --vm --disk-util --disk-tps --freespace --swap --top-io --top-bio-adv

```

There's probably a shorter way to write it but then again, shell history or aliases.

### GDB

This one is a rather complicated and powerful tool, but I'm only covering the basic stuff (setup and basic commands).

```
sudo apt-get install gdb python-dbg
zcat /usr/share/doc/python2.7/gdbinit.gz > ~/.gdbinit
run app with python2.7-dbg
sudo gdb -p 12345

```

Now use:

- bt - stacktrace (C level)
- pystack - python stacktrace, you need to have *~/.gdbinit* and use python-dbg unfortunately
- c (continue)

### Worthy mentions

- [sysdig](http://www.sysdig.org/) - like strace and lsof but with superpowers.

## Having segfaults? faulthandler

Rather awesome addition from Python 3.3, [backported](https://pypi.python.org/pypi/faulthandler/) to Python 2.x

Just do this and you'll get at least an idea of what's causing the segmentation fault. Just add this in some module that's always imported:

```
import faulthandler
faulthandler.enable()

```

This won't work in [PyPy](http://pypy.org/) unfortunately. If you can't get interactive (e.g.: use gdb) you can just set this environment variable (GNU libc only, [details](http://blog.andrew.net.au/2007/08/15/)):

```
export LD_PRELOAD=/lib/x86_64-linux-gnu/libSegFault.so

```

Make sure the path is correct - otherwise it won't have any effect (e.g.: run locate libSegFault.so).

## Quick stacktrace on a signal? faulthandler

Add this in some module that's always imported:

```
import faulthandler
import signal
faulthandler.register(signal.SIGUSR2, all_threads=True)

```

Then run kill -USR2 <pid> to get a stacktrace for all threads on the process's *stderr*.

## Memory leaks

Well, there's are plenty of tools here, some specialized on WSGI applications like [Dozer](https://pypi.python.org/pypi/Dozer) but my favorite is definitely [objgraph](https://pypi.python.org/pypi/objgraph). It's so convenient and easy to use it's amazing. It's doesn't have any integration with WSGI or anything so you need to find yourself a way to run code like:

```
>>> import objgraph
>>> objgraph.show_most_common_types() # try to find objects to investigate
Request                  119105
function                   7413
dict                       2492
tuple                      2396
wrapper_descriptor         1324
weakref                    1291
list                       1234
cell                       1011
>>> objs = objgraph.by_type("Request")[:15] # select few Request objects
>>> objgraph.show_backrefs(objs, max_depth=15, highlight=lambda v: v in objs, filename="/tmp/graph.png") # and plot them
Graph written to /tmp/objgraph-zbdM4z.dot (107 nodes)
Image generated as /tmp/graph.png

```

And you get a nice diagram like [this](https://blog.ionelmc.ro/2013/06/05/python-debugging-tools/objgraph-plot.png) (warning: it's very large). You can also get [dot](http://www.graphviz.org/content/dot-language) output.

## Memory usage

Sometimes you want to use less memory. Less allocations usually make applications faster and well, users like them lean and mean :)

There are lots of tools [[\]](https://blog.ionelmc.ro/2013/06/05/python-debugging-tools/#id4) but the best one in my opinion is [pytracemalloc](https://github.com/wyplay/pytracemalloc) - it has very little overhead (doesn't need to rely on the speed crippling [sys.settrace](http://docs.python.org/2/library/sys.html#sys.settrace)) compared to other tools and it's output is very detailed. It's a pain to setup because you need to recompile python but apt makes it very easy to do so. In fact, it is *so good* that it [got included in Python 3.4](http://docs.python.org/3.4/library/tracemalloc.html). See [PEP-454](http://www.python.org/dev/peps/pep-0454/) for details.

Just run these commands and go grab lunch or something:

```
apt-get source python2.7
cd python2.7-*
wget https://github.com/wyplay/pytracemalloc/raw/master/python2.7_track_free_list.patch
patch -p1 < python2.7_track_free_list.patch
debuild -us -uc
cd ..
sudo dpkg -i python2.7-minimal_2.7*.deb python2.7-dev_*.deb

```

Alternativelly, you can use this [ppa](https://launchpad.net/%7Eionel-mc/+archive/pytracemalloc) but I think it might be outdated by now. You can make your own ppa, it's [easy enough](https://gist.github.com/ionelmc/7109195).

And install pytracemalloc (note that if you're doing this in a virtualenv, you need to recreate it after the python re-install - just run virtualenv myenv):

```
pip install pytracemalloc

```

Now wrap your application in code like this:

```
import tracemalloc, time
tracemalloc.enable()
top = tracemalloc.DisplayTop(
    5000, # log the top 5000 locations
    file=open('/tmp/memory-profile-%s' % time.time(), "w")
)
top.show_lineno = True
try:
    # code that needs to be traced
finally:
    top.display()

```

And output is like this:

```
2013-05-31 18:05:07: Top 5000 allocations per file and line
#1: .../site-packages/billiard/_connection.py:198: size=1288 KiB, count=70 (+0), average=18 KiB
#2: .../site-packages/billiard/_connection.py:199: size=1288 KiB, count=70 (+0), average=18 KiB
#3: .../python2.7/importlib/__init__.py:37: size=459 KiB, count=5958 (+0), average=78 B
#4: .../site-packages/amqp/transport.py:232: size=217 KiB, count=6960 (+0), average=32 B
#5: .../site-packages/amqp/transport.py:231: size=206 KiB, count=8798 (+0), average=24 B
#6: .../site-packages/amqp/serialization.py:210: size=199 KiB, count=822 (+0), average=248 B
#7: .../lib/python2.7/socket.py:224: size=179 KiB, count=5947 (+0), average=30 B
#8: .../celery/utils/term.py:89: size=172 KiB, count=1953 (+0), average=90 B
#9: .../site-packages/kombu/connection.py:281: size=153 KiB, count=2400 (+0), average=65 B
#10: .../site-packages/amqp/serialization.py:462: size=147 KiB, count=4704 (+0), average=32 B

...

```

Beautiful, no?

[pytracemalloc alternatives](https://github.com/wyplay/pytracemalloc#see-also)

EDIT: More about profiling [here](https://blog.ionelmc.ro/2013/06/08/python-profiling-tools).

This entry was tagged as [python](https://blog.ionelmc.ro/tag/python/)[debugging](https://blog.ionelmc.ro/tag/debugging/)




