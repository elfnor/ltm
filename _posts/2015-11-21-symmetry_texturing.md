---
author: elfnor
date: '2015-11-21 22:00'
image: 'blender\_sym\_tile\_p6m.png'
layout: post
tags: ' blender symmetry-tile'
title: Symmetry Tile Painting in Blender
---

After working out how to use Blender to paint [hyperbolic tilings]({{ site.baseurl }}{% link _posts/2015-11-14-hyperbolic_texturing.md %}) I then thought of using Blender to look at planar symmetric tilings.

A while back I wrote a [GIMP plugin]({{ site.baseurl }}{% link _posts/2014-07-05-symmetry_tile_docs.md %}) which takes a selection from an image ("cell") and produces a new image according to one of the [17 plane symmetry groups](http://en.wikipedia.org/wiki/Wallpaper_groups). These are also known as wallpaper groups or plane crystallographic groups. Basically it rotates and or flips copies of the cell, combines them to form a tile and then copies that tile to fill a new image. The GIMP plugin dosen\'t update in real time. If you want to make changes you need to re-run the plugin to generate a new tiling.

Using UV mapping its really easy to get Blender to draw tilings from any of the 17 plane symmetry groups that update as you paint.

This blend file ([sym\_tile.blend](/downloads/sym_tile.blend)) has been set up with 17 plane grids each on a separate layer. Each layer implements one of the symmetry groups via a UV map.

Here\'s a screencast that shows the live drawing in action and explains how I set up the UV maps.

<iframe width="660" height="420" src="http://www.youtube.com/embed/ILBDlT9oRNI?autoplay=0">
</iframe>
[Download Link for HD version](/downloads/sym_tile.mp4)

For more on plane symmetry groups see the [galleries](http://elfnor.github.io/symmetrytilegallery) of images produced with the GIMP plugin, my post on [symmetry group notation]({{ site.baseurl }}{% link _posts/2014-07-18-symmetry_group_notation.md %}) which also has links to other online resources and my post on [quilt design]({{ site.baseurl }}{% link _posts/2014-07-12-symmetry_tile_quilt_design.md %}).

------------------------------------------------------------------------
