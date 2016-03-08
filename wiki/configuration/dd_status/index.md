---
layout: page
title: dd_status
math_support: mathjax
---


linux command dd
`[sudo] dd if=blabla of=blabla bs=1M status=progress`

on mac seems status parameter can not be specified explicitly

`[sudo] dd if=blabla of=blabla bs=1M` this is enough

send SIGINFO signal to see how it works now

~~~bash
pgrep -l '^dd$'
kill -SIGINFO 8789
watch -n 10 kill -USRINFO 8789
~~~

reference
[show progress during dd](http://linuxcommando.blogspot.com/2008/06/show-progress-during-dd-copy.html)


