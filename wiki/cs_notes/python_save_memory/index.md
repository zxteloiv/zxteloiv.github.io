---
layout: page
title: python_save_memory
math_support: mathjax
---


### use sqlite3 in memory to deal with data and thus save memory used by python objects

sqlite3.connect(':memory:')

### redis

a redis server

### mmap to map file / device into memory

https://en.wikipedia.org/wiki/Mmap
http://man7.org/linux/man-pages/man2/mmap.2.html
https://docs.python.org/2/library/mmap.html

### other modules

python dict -> list -> set -> bitarray on pypi

bit manipulation:
https://wiki.python.org/moin/BitManipulation

an recipy 
~~~ python
#
# bitfield manipulation
#

class bf(object):
    def __init__(self,value=0):
        self._d = value

    def __getitem__(self, index):
        return (self._d >> index) & 1 

    def __setitem__(self,index,value):
        value    = (value&1L)<<index
        mask     = (1L)<<index
        self._d  = (self._d & ~mask) | value

    def __getslice__(self, start, end):
        mask = 2L**(end - start) -1
        return (self._d >> start) & mask

    def __setslice__(self, start, end, value):
        mask = 2L**(end - start) -1
        value = (value & mask) << start
        mask = mask << start
        self._d = (self._d & ~mask) | value
        return (self._d >> start) & mask

    def __int__(self):
        return self._d

k = bf()
k[3:7]=5
print k[3]
print k[5]
k[7]=1
print k[4:8]
print int(k)

~~~

### reason investigation

from http://stackoverflow.com/questions/10264874/python-reducing-memory-usage-of-dictionary

I cannot offer a complete strategy that would help improve memory footprint, but I believe it may help to analyse what exactly is taking so much memory.

If you look at the **Python implementation** of dictionary (which is a relatively straight-forward implementation of a hash table), as well as the implementation of the built-in string and integer data types, for example [here](http://svn.python.org/view/python/trunk/Include/) (specifically object.h, intobject.h, stringobject.h and dictobject.h, as well as the corresponding *.c files in ../Objects), you can calculate with some accuracy the expected space requirements:

1. An integer is a fixed-sized object, i.e. it contains a reference count, a type pointer and the actual integer, in total typically at least 12 bytes on a 32bit system and 24 bytes on a 64bit system, not taking into account extra space possibly lost through alignment.
2. A string object is variable-sized, which means it contains
   - reference count
   - type pointer
   - size information
   - space for the lazily calculated hash code
   - state information (e.g. used for interned strings)
   - a pointer to the dynamic content
   in total at least 24 bytes on 32bit or 60 bytes on 64bit, not including space for the string itself.
3. The dictionary itself consists of a number of buckets, each containing
   - the hash code of the object currently stored (that is not predictable from the position of the bucket due to the collision resolution strategy used)
   - a pointer to the key object
   - a pointer to the value object
   in total at least 12 bytes on 32bit and 24 bytes on 64bit.
4. The dictionary starts out with 8 empty buckets and is resized by doubling the number of entries whenever its capacity is reached.

I carried out a test with a list of 46,461 unique strings (337,670 bytes concatenated string size), each associated with an integer — similar to your setup, on a 32-bit machine. According to the calculation above, I would expect a minimum memory footprint of

- 46,461 * (24+12) bytes = 1.6 MB for the string/integer combinations
- 337,670 = 0.3 MB for the string contents
- 65,536 * 12 bytes = 1.6 MB for the hash buckets (after resizing 13 times)

in total 2.65 MB. (For a 64-bit system the corresponding calculation yields 5.5 MB.)

When running the Python interpreter idle, its footprint according to the ps-tool is 4.6 MB. So the total expected memory consumption after creating the dictionary is approximately 4.6 + 2.65 = 7.25 MB. The true memory footprint (according to ps) in my test was 7.6 MB. I guess the extra ca. 0.35 MB were consumed by overhead generated through Python's memory allocation strategy (for memory arenas etc.)

Of course many people will now point out that my use of ps to measure the memory footprint is inaccurate and my assumptions about the size of pointer types and integers on 32-bit and 64-bit systems may be wrong on many specific systems. Granted.

But, nevertheless, the key conclusions, I believe, are these:

- The Python dictionary implementation consumes a surprisingly small amount of memory
- But the space taken by the many int and (in particular) string objects, for reference counts, pre-calculated hash codes etc., is more than you'd think at first
- There is hardly a way to avoid the memory overhead, as long as you use Python and want the strings and integers represented as individual objects — at least I don't see how that could be done
- It may be worthwhile to look for (or implement yourself) a Python-C extension that implements a hash that stores keys and values as C-pointers (rather than Python objects). I don't know if that exists; but I believe it could be done and could reduce the memory footprint by more than half.





