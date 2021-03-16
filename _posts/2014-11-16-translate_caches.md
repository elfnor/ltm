---
author: elfnor
date: '2014-11-16 22:00'
layout: post
tags:  geocaching
title: Translate cache descriptions and logs
permalink: translate-cache-descriptions-and-logs.html
---

I recently did a European cycle trip and as described [here]({{ site.baseurl }}{% link _posts/2014-11-09-caches_along_track.md %}) I made some gpx files of geocaches along my intended route.

The trouble is most of the cache descriptions were in German and my German is practically non-existent. I wanted to do all my caching offline so I wrote a small python script to use Google Translate to translate all the descriptions and logs in the GPX file.

The code uses the `minidom` module in the python standard library to parse the `xml` in the GPX file.

It uses the [goslate](http://pythonhosted.org/goslate/) library to query Google translate. The `codecs` module also in the python standard library is used to correctly write out the unicode characters.

The script attempts to translate the description and logs for each geocache into English and append them after the original text.

WARNING: This code only translates some descriptions and logs, if there is a lot of formatting in the description it fails to translate and just returns a second copy of the original text. I haven\'t spent much time trying to resolve this bug. Feel welcome to improve the script for your on use. Also use at your own discretion after reading Google Translate\'s terms and conditions.

```python
from xml.dom import minidom
import codecs
import goslate
import time

gs = goslate.Goslate()
fn = 'route-caches.gpx'


xmldoc = minidom.parse(fn)    
descs =  xmldoc.getElementsByTagName('groundspeak:long_description')

n = len(descs)    
    
for i, node in enumerate(descs):
    try:
        desc = node.firstChild.nodeValue
        time.sleep(0.2) 
        endesc =  gs.translate(desc, 'en')
        node.firstChild.nodeValue  = (desc + ' <hr> ' + endesc)
    except:
        pass
        
    print '%d of %d' %(i, n)
    
    
texts = xmldoc.getElementsByTagName('groundspeak:text')
n = len(texts)    
    
for i, node in enumerate(texts):
    try:
        text = node.firstChild.nodeValue
        time.sleep(0.2) 
        entext =  gs.translate(text, 'en')
        node.firstChild.nodeValue  = (text + ' <hr> ' + entext)
    except:
        pass
        
    print '%d of %d' %(i, n)

with codecs.open('route-caches-en.gpx', "w", "utf-8") as out:
    xmldoc.writexml(out)
```

------------------------------------------------------------------------
