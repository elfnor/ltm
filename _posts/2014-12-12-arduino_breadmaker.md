---
author: elfnor
category: make
date: '2014-12-12 22:00'
image: 'arduino\_bread.png'
layout: post
slug: Arduino Bread
title: Arduino Bread
---

I\'m working on a project to replace the \"brain\" of a bread-maker with an Arduino.

My partner makes all the household\'s bread from a sourdough starter. He uses the current bread-maker to knead the bread and sometimes to bake the bread but a bread-maker with its fixed cycle doesn\'t\'t make a successful loaf of sourdough. The rising time for bread made with sourdough starter is variable and unpredictable. Also the standard bread-maker cycle has a \"punch down\" (a quick mix in the middle of the rising cycle). This helps to make a fine texture for ordinary bread but can disrupt the rising of a sourdough loaf.

By replacing the bread-maker control circuit with an arduino, we\'ll have more control over the stages in the bread-maker cycle. For instance a leave to rise until the human baker switches to the bake cycle will cover the variation in rise time we often get with the sourdough. Eventually, I\'d like some way of measuring the height of the dough so the arduino can track when the bread is risen and then switch to the bake cycle.

Also a programmable temperature environment with optional mixing has potential for other projects. Pasta dough, jam, dyeing wool, risotto, [beer making](https://www.youtube.com/watch?v=wgUw5Sj5HK0), [roasting coffee](http://hackaday.com/2010/01/29/another-take-on-roasting-those-beans/), [compost](http://www.instructables.com/id/Mr-Compost-How-to-make-an-in-kitchen-compost-tur/), desoldering etc.

Note : don\'t try these in the same bread-maker or at least don\'t make the jam after trying the desolder or compost. Note I only found the compost making instructable after I\'d mostly finished my version, (for some reason I didn\'t search for compost and bread machine) but it looks a great place to start. What\'s interesting is the basic board layout in the compost making Zojirushi BBCC-V20 looks similar internally to the Sanyo \"The Bread Factory Plus\" (Model SBM-20) I\'m working on.

I\'ll post some detailed notes when I\'m a bit further along in the project. But so far I\'ve managed to duplicate the basic bread-making cycle on the arduino. Here\'s the proof. Arduino baked bread.

------------------------------------------------------------------------
