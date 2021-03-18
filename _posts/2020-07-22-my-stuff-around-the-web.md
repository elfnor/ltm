---
author: elfnor
date: '2020-07-11'
image: 'abstract_21.png'
layout: post
tags: blender generative-art maze copy2 structure-synth
title: 'Update on old projects'
---

It's been a while since I've regularly updated this blog (blame the day job and life). Here's some updates and links on stuff related to addons I've made for Blender or scripted nodes for Sverchok.

## Generative Art in Sverchok for Blender

The node I wrote to do 3D L-systems in Blender via the [Sverchok add-on](http://nikitron.cc.ua/sverch/html/main.html) is now a node in the main release of Sverchok.  It has been kept updated  by others on the Sverchok team and works with the latest Blender. It's now in Sverchok under `Nodes > Generator > Generative Art` See the [offical docs](https://sverchok.readthedocs.io/en/latest/nodes/generators_extended/generative_art.html)

My work was inspired by [Structure Synth](http://structuresynth.sourceforge.net/). Kronpano has  made  [BrowserSynth](https://github.com/kronpano/BrowserSynth) an online implementation of Structure Synth. It uses the original eisenscript notation.

[Humphrey Hippo](https://humphreyhippo.wordpress.com/) had a number of blog posts on Generative Art in Sverchok. 

See his [structure-synth tag](https://humphreyhippo.wordpress.com/category/structure-synth/) for a full list but particularly:

* [Adventures in Sverchok’s Generative Art Node](https://humphreyhippo.wordpress.com/2017/12/13/adventures-in-sverchoks-generative-art-node/)
* [Preparing Eisenscript for the XML Translator](https://humphreyhippo.wordpress.com/2017/12/20/preparing-eisenscript-for-the-xml-translator/)
* [Photo 51 – Structure Synth DNA](https://humphreyhippo.wordpress.com/2018/02/20/photo-51-structure-synth-dna/)

[My original posts]({{ site.baseurl }}{% link tag/sverchok.md %})

If you want to learn Sverchok [codeplastic's ebook](http://www.codeplastic.com/learning-sverchok-ebook/) looks like a great place to start. 

## Mesh Maze Blender Add-on

I've updated my Blender [mesh maze add-on](https://github.com/elfnor/mesh_maze) (the non-Sverchok version) to Blender 2.8. This can generate a maze pattern on the surface of any mesh

Steven Scott has done a [you-tube tutorial](https://www.youtube.com/watch?v=wVnp6R8BDfo) on installing and using it.

There's also a [tutorial](https://blender-addons.org/mesh-maze-addon/) on this blender-addons site.

[My original post]({% link _posts/2017-06-25-blender_mesh_maze_addon.md %})

## Copy2 Blender Add-on

I've also updated the [Copy2 Blender add-on](https://github.com/elfnor/copy2_blender_addon) to Blender 2.8. This add-on can copy one object to the vertices or faces or edges of another object. The direction of the copy from object is set by the normals of the copy to object.

Steven Scott has done a [you-tube tutorial](https://www.youtube.com/watch?v=wYYFSTHCXsk)on this one as well. 

[My original post]({% link _posts/2014-09-02-copy2_docs.md %})

## Auto Save Render Blender Add-on

I wrote a Blender add-on to automatically save a copy of the Blender file along with a copy of the rendered image each time a Blender file was rendered. This is a crude form of version control.  Most of the functionailty of my version is now incorporated in the original by [Florian Felix](https://github.com/florianfelix)  which often appears under the community or testing tabs in `Edit > Preferences > Addons` but not always in the final releases. The code is on the developer website at https://developer.blender.org/diffusion/BAC/browse/master/render_auto_save.py and is more up-to-date than mine. It works in Blender 2.83. 

[My original post]({% link _posts/2018-03-31-blender_auto_save_addon_update.md %})

## Tree Generator node for Sverchok

My [tree generator scripted node](https://github.com/elfnor/spacetree-sverchok) for Sverchok uses the space colonization algorithim to generate branched structures from a cloud of points. I've added a scripted node lite version to the repo that works with Sverchok 0.6 and Blender 2.83.

There's a few videos by Jimmy Gunawan showing its use:

* [# LIVENODING 846 / SV Mirror Metaballs of Elfnor Tree](https://www.youtube.com/watch?v=VL3Z5FDq8XE)
* [# BLENDERSUSHI / SV Elfnor Space Tree Maze Design (LIVENODING379)](https://www.youtube.com/watch?v=pqfIjWlTIqM)
* [# BLENDERSUSHI / SV Elfnor Tree Generator (LIVENODING068)](https://www.youtube.com/watch?v=M2QVSoN_51c)

[My original post]({% link _posts/2016-08-12-tree_generator.md %})

## Conway polyhedra

I implemented Conway's polyheron operators as a [set of Sverchok scripted nodes](https://github.com/elfnor/conway_polyhedron_operators). They can be used to generate complex polyhedra. I've updated this to work in Blender 2.8,  but I haven't throughly tested that update yet.

Andrew Marsh has an [online web app](http://andrewmarsh.com/software/poly3d-web/) for playing with the operators.

[My original post]({% link _posts/2017-12-18-conway_polyhedron_operators.md %})
