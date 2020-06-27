---
author: elfnor
category: think
date: '2015-10-02 22:00'
image: 'dragon\_fruit\_vertex\_color\_05\_009.png'
layout: post
tags:  blender sverchok
title: 'Shaders for Sverchok 04 - Vertex Colors Update'
---

When I wrote my [Shaders for Sverchok 02 - Vertex Colors](%7Bfilename%7Dshady_sverchok_02.md) post, I used a small piece of python code to assign the vertex colors. Sverchok now has a \"Vertex Colors\" node and I\'ll now show how to use that instead.

The \"Vertex Color\" node takes a list of colors as input. This list can either be assigned to the vertices \"v\" or to the faces \"p\" of the object selected in the drop down. Each color should be a vector of three numbers between 0 and 1. If the list is shorter that the number of vertices (or polygons) Sverchok will do its normal thing and very usefully repeat the list until it is long enough.

The second drop down on the node is used to select the vertex color layer of the object.

The example below makes up a list of 6 random colors and applies them to the faces of the stack of cubes. The default list repeat thing means the colors are repeated up the stack and the colors are repeated on the same faces. Make sure to use a different \"Seed\" for each of three random number generators. If they all have the same \"Seed\" they will produce the same set of numbers for each color channel and the colors will all be shades of grey.

Make sure you select the object and switch the view to \"Vertex Paint\" mode to see the vertex colors. To change the vertex colors into material colors that appear in a render you need to use a **Material** node tree as in [this post](%7Bfilename%7Dshady_sverchok_02.md).

![nodes for vertex color example 1]({{ site.baseurl }}/images/vertex_color_cube_example_01.png)

If instead we want to apply a separate color to each cube in the stack we need to reorder our list of colors. I\'m still working on understanding all the list manipulation nodes in Sverchok but I wrangled my list of 5 colors (one color per cube) using the \"List Match\" node. It takes a list of integers the same length as the number of vertices per cube and combines it (using the \'X-Ref\" option) with a list of 5 colors. Use the \"Viewer Text\" node to work out how the various options work

![nodes for vertex color example 2]({{ site.baseurl }}/images/vertex_color_cube_example_02.png)

The version of the \"Vertex Color\" node I\'m using is available [here](/downloads/colors.py). I\'ll do a pull request for it to be included in the master branch of Sverchok. With this node as well as being able to set the vertex color layer of an object I\'ve added the ability to read the colors. This is similar to the way the \"Vertex Weights\" node already works.

With this version of the node you can paint the vertex colors onto one object and then transfer them to copies of the object. In the example below the small \"fruit segment\" object was painted with red and green vertex colors. The mesh of the object is copied to the polygon centers of the bloom sphere to produce the \"Alpha\" object. The two \"Vertex Colors\" nodes are then used to copy the vertex colors from the \"fruit segment\" object to the \"Alpha\" object.

![dragon fruit nodes]({{ site.baseurl }}/images/dragon_fruit_nodes.png)

------------------------------------------------------------------------
