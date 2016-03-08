---
layout: page
title: Port_Exhausted_When_Accessing_Local_Server
math_support: mathjax
---


File "/home/zxteloiv/local/python27/lib/python2.7/site-packages/requests/adapters.py", line 437, in send
          raise ConnectionError(e, request=request)
        requests.exceptions.ConnectionError: HTTPConnectionPool(host='127.0.0.1', port=9000): Max retries exceeded with url: /?properties=%7B%27outputFormat%27%3A+%27json%27%2C+%27annotators%27%3A+%27tokenize%2Cssplit%2Cpos%27%7D (Caused by NewConnectionError('<requests.packages.urllib3.connection.HTTPConnection object at 0x7fe7c17b7910>: Failed to establish a new connection: [Errno 99] Cannot assign requested address',))

[stackoverflow.com/questions/11981933/](http://stackoverflow.com/questions/11981933/python-urllib2-cannot-assign-requested-address)

solve:
~~~bash
sudo sysctl net.ipv4.tcp_tw_recycle=1
sudo sysctl net.ipv4.tcp_max_orphans=8192
sudo sysctl net.ipv4.tcp_orphan_retries=1
~~~


