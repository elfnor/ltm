---
author: elfnor
date: '2016-05-29 22:00'
image: 'medusa\_spiked\_01\_016.png'
layout: post
tags: ' blender sverchok structure-synth generative-art'
title: Structure Synth eisenscript to xml translator
---

I\'ve been working on some code to automatically translate the original *eisenscript* files used by [Struture Synth](http://structuresynth.sourceforge.net/) into the *eisenxml* used by the [Generative Art node](%7Bfilename%7Dgenerative_art_docs.md) in the [Sverchok](https://github.com/nortikin/sverchok) addon for [Blender](https://www.blender.org/).

Its not complete but I\'ve uploaded the work so far to [github](https://github.com/elfnor/generative-art-examples/blob/master/eisenscript_to_xml.py). Its written using [pyparsing](http://pyparsing.wikispaces.com/). This is my first serious attempt at using pyparsing. I was impressed with how far I could get with this project so quickly. My code is pretty ugly and could still do with some serious refactoring work, but it translates all the examples provided with Structure Synth with the exception of those using preprocessor (\#define) commands.

After writing most of the pyparsing grammar based on the Structure Synth reference [page](http://structuresynth.sourceforge.net/reference.php) I found the basics of [EBNF notation](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_Form) used for eisenscript in the [source files](https://sourceforge.net/p/structuresynth/code/HEAD/tree/trunk/notes.txt#l48).

The translator completely ignores any eisenscript commands to do with color, setting the camera or the ray tracer as those are better dealt with in Blender outside Sverchok.

I haven\'t translated the preprocessor commands (\#define). These commands are similar to the way I\'ve used constants and variables in the Sverchok node but different enough to be a bit harder to implement in the translator.

So far its translating most things I\'ve thrown at it and the geometry in Blender looks mostly equivalent to the original Structure Synth geometry. It hasn\'t been systematically tested and there\'s bound to be valid eisenscript in can\'t translate properly. As is where is.

To use the [translator](https://github.com/elfnor/generative-art-examples/blob/master/eisenscript_to_xml.py):

    $ python eisenscript_to_xml.py /path/to/file/totranslate.es

will produce a file `/path/to/file/totranslate.es.xml`

Here\'s the original eisenscript and translated eisenxml for one of the creatures above.

![ss\_medusa]({{ site.baseurl }}/images/ss_medusa.png)

    set md 25

    {ry 90 hue 35 sat 0.5} body

    rule body {
    20 * {rx 18} segment
    }

    rule segment md 12 > arc1 {
    box
    {z 1 ry 10 s 1.2} segment
    }

    rule arc1 {
    {s 0.6   x -7 z 11} arc
    }

    rule arc {
    5 * {ry 10 } subarc
    }

    rule subarc {
    {x 15} spike
    }

    rule spike  w 5 {
    10 * {z 0.9 ry 5 s 0.9} sphere
    }

    rule spike w 5 {
    10 * {z 0.9 ry -5 s 0.9} sphere
    }

    rule spike w 1 {
    10 * {z 0.9 ry 10 s 0.9} sphere
    }

    rule spike w 1 {
    10 * {z 0.9 ry -10 s 0.9} sphere
    }

![ga\_medusa]({{ site.baseurl }}/images/ga_medusa.png)

```xml
<?xml version="1.0" ?>
<rules max_depth="25">
  <rule name="entry">
    <call rule="body" transforms="ry 90"/>
  </rule>
  <rule name="body">
    <call count="20" rule="segment" transforms="rx 18"/>
  </rule>
  <rule max_depth="12" name="segment" successor="arc1">
    <instance shape="box" transforms=""/>
    <call rule="segment" transforms="tz 1 ry 10 sa 1.2"/>
  </rule>
  <rule name="arc1">
    <call rule="arc" transforms="sa 0.6 tx -7 tz 11"/>
  </rule>
  <rule name="arc">
    <call count="5" rule="subarc" transforms="ry 10"/>
  </rule>
  <rule name="subarc">
    <call rule="spike" transforms="tx 15"/>
  </rule>
  <rule name="spike" weight="5">
    <instance count="10" shape="sphere" transforms="tz 0.9 ry 5 sa 0.9"/>
  </rule>
  <rule name="spike" weight="5">
    <instance count="10" shape="sphere" transforms="tz 0.9 ry -5 sa 0.9"/>
  </rule>
  <rule name="spike" weight="1">
    <instance count="10" shape="sphere" transforms="tz 0.9 ry 10 sa 0.9"/>
  </rule>
  <rule name="spike" weight="1">
    <instance count="10" shape="sphere" transforms="tz 0.9 ry -10 sa 0.9"/>
  </rule>
</rules>
```

Feel free to post links in the comments to any eisenscript files that break the translator and I\'ll (probably) have a look at them.
