---
layout: page
title: Crawler Notes
math_support: mathjax
---


## framework or not?

If you need one or many of the followings, consider using a framework:

- url queue
- congest control
- flow control
- concurrency control
- xpath / css selector
- restart at the break point

when no framework, just like original python and use requests to crawl which is just part of the script's work  

## javascript rendered webpages

PhantomJS (node) and selenium (python) are pratical methods for *real* rendering a webpage (of course without user interaction). Using this webpage we could go on.

But it's better to find some http API used by the rendering javascript, which is much easier.

## restart and restore the states where it is stopped

although scrapy can be shutdown by sending C-c SIGINT signal, it will continue processing the item already in queue.

Use a middleware to detect whether the data is already in. and restart every time. refer to scrapy download middleware documents for more detail.

~~~ python
import os
import scrapy
import re
import json

class MyDownloaderMiddleware(object):
    def process_request(self, request, spider):
        itemid = ''
        urlparts = request.url.split('/')
        if 'view' in urlparts:
            itemid = re.search('(\d+).htm', urlparts[-1]).group(1) + ".htm"
        elif 'subview' in urlparts:
            itemid = "_".join(re.search('(\d+)', x).group(1) for x in urlparts[-2:]) + ".htm"
        if itemid:
            savedir = spider.settings.get("BAIKE_HTML_SAVE_DIR", default=".")
            for parent,dirnames,filenames in os.walk(savedir):
                if itemid in filenames:
                    with open(os.path.join(savedir, itemid), 'r') as f:
						s = f.readlines()
						f.close()
                    return scrapy.http.HtmlResponse(url=json.loads(s[0])['url'],body=''.join(s[1:]))
                else:
                    return None
        else:
            return None
        
~~~

and modify the settings:

~~~ python
DOWNLOADER_MIDDLEWARES = {
    'bk.middlewares.MyDownloaderMiddleware': 543,
}
~~~


