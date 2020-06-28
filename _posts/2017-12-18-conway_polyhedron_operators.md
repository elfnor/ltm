---
author: elfnor
date: '2017-12-18 22:00'
image: 'conway\_img\_03\_017.png'
layout: post
tags:  blender sverchok
title: Conway Polyhedron Operators in Sverchok
use_math: true
---

I was working with abstract sculptures in Blender based on polyhedra and was getting annoyed that the Regular Solids (part of the [Extra Objects](https://wiki.blender.org/index.php/Extensions:2.6/Py/Scripts/Add_Mesh/Add_Extra) add-on) still divides polyhedron faces into quads and tris rather than using ngons for pentagonal faces for example. A quick look at the source code and it was obvious how to change this. But I started an Internet search on how the snub polyhedron were formed, this led to [Conway Polyhedra](https://en.wikipedia.org/wiki/Conway_polyhedron_notation) and I was deep in a [rabbit hole](https://www.urbandictionary.com/define.php?term=Rabbit%20Hole).

from [Wikipedia](https://en.wikipedia.org/wiki/Conway_polyhedron_notation)

![conway operators]({{ site.baseurl }}/images/conway_examples.png)

Conway Polyhedra are formed by applying various operators to a seed polyhedron such as one of the platonic solids. Some of the operators are similar to modifiers already present in Blender or nodes already in [Sverchok](https://github.com/nortikin/sverchok/). For example the *ambo* Conway operator is the same as the *Bevel* modifier with *Only vertices* checked and a *Width* of 1.0. The *kis* Conway operator is equivalent to *Mesh \> Faces \> Poke Faces* in edit mode.

Other of the Conway operators are more challenging to reproduce in Blender and Sverchok. I went searching expecting to find a python implementation of the Conway operators I could adapt to Sverchok but no luck. There is a very good online [coffeescript app](https://github.com/levskaya/polyhedronisme) and Kit Wallace has a great [openscad implementation](https://github.com/KitWallace/openscad/blob/master/conway.scad)

So I\'ve written a [module](https://github.com/elfnor/conway_polyhedron_operators) to implement a subset of the Conway operators in python. I\'ve made the only dependency Blender\'s mathutils library. This means the code can be run outside Blender using a [standalone version of the mathutils module](https://github.com/majimboo/py-mathutils).

## Usage Notes

The *conway.py* module is designed to be used in Blender with Sverchok\'s *Scripted Node Lite*.

-   Install [Sverchok](https://github.com/nortikin/sverchok/) in [Blender](https://www.blender.org/).
-   Download and unzip (or clone) from the [repo](https://github.com/elfnor/conway_polyhedron_operators) on github
-   Open *conway.py* as a text block in Blender.
-   Open *snl\_plato.py* and *snl\_conway\_op.py* as text blocks in Blender. These contain code for each Sverchok *Scripted Node Lite*.
-   In a *Node Editor* view create a new *Node Tree* and add two *Scripted Node Lite* nodes.
-   Use the notebook icon on the node to select *snl\_plato.py* on the left node and *snl\_comway\_op.py* on the right node. Click the plug icon on each node to load the code.
-   Wire up the nodes along with a *Viewer Draw* node as shown below.

![conway nodes]({{ site.baseurl }}/images/conway_aD.png)

Wire up multiple copies of *snl\_conway\_op.py* in a row to produce more complex shapes.

![conway\_aagD]({{ site.baseurl }}/images/conway_aagD.png)

Two of the operators *kis* and *chamfer* can take parameters such as the height of the *kis* pyramid or the *height* and *thickness* of the *chamfer*. There is a separate *Scripted Node Lite* given for these two operators with sliders for the parameters.

Some operators, particularly *gyro*, *propellor* and *whirl* and *chamfer* give polyhedra that are not particularly smooth or convex, the faces may not be flat or symmetric.

![conway cgC]({{ site.baseurl }}/images/conway_cgC.png)

The canonical form of a convex polyhedra has all faces planar and all edges tangential to the unit sphere. The centre of gravity of the tangential points is also at the centre of the same unit sphere.

The module *canon.py* contiains functions that attempt to shift the points of a polyhedron to satisfy these conditions. This is a iterative process and can take several hundred steps to converge.

To try this in Sverchok, add the *canon.py* and *snl\_canon.py* files as text blocks in your Blender file and add *snl\_canon.py* as a *Scripted Node Lite*. The node has two parameters *iterations* and *scale\_factor*. At each iteration the vertices are moved a *scale\_factor* fraction of the calculated distance. Setting this parameter too high may cause the shape to become unstable. Increasing the *iterations* will increase the calculation time.

![conway\_CcgC.png]({{ site.baseurl }}/images/conway_CcgC.png)

The canonicalization can also be applied after each operator. In the example below just enough iterations have been applied to form a pleasing shape. The proper canonical form of this polyhedra should be the same whether the canonicalization is performed once or twice.

![conway\_CcCgC.png]({{ site.baseurl }}/images/conway_CcCgC.png)

These Conway operators can be applied to any manifold (ie. a closed solid) mesh not just the platonic solids. They currently don\'t work on planar grids unless one applies a solidify node to the grid first.

![conway\_kg\_hexa\_grid]({{ site.baseurl }}/images/conway_kg_hexa_grid.png)

Other Sverchok nodes of course can be used interspersed with the Conway operators for other effects.

I\'ve only implemented a subset of the operators defined on the Wikipedia page. Many of the operators are equivalent to a combination of other operators as shown in the chart

### Conversion chart

The operator order is given as the left to right node order. Note that this is the opposite to the order given in the Conway notation.

  Operator    Description                                    Implementation
  ----------- ---------------------------------------------- --------------------
  kis         poke face                                      node
  dual        faces become vertices, vertices become faces   node
  ambo        full vertex bevel                              node
  chamfer     hexagons replace edges                         node
  gyro        faces divided into pentagons                   node
  whirl       insets a smaller rotated copy of the face      node
  propellor   insets a rotated copy of the face              node
  zip         dual of kis                                    kis dual
  expand      edge bevel                                     ambo ambo
  bevel       vertex bevel applied twice                     ambo dual kis dual
  snub        dual of gyro                                   gyro dual
  join        dual of ambo                                   ambo dual
  needle      dual of truncate                               dual kis
  ortho       single subdivide                               ambo ambo dual
  meta        poke face and subdivide edges                  ambo dual kis
  truncate    half vertex bevel                              dual kis dual

## Future

Ironically I haven\'t coded the *snub* operator directly, and I\'ve used the code from *add\_mesh\_extra\_objects.add\_mesh\_solid* to implement the platonic solids with n-gons.

Some of these operators would be useful as full nodes in Sverchok, particularly *dual*.

Make the operators work on open edge meshes.

Another way of making the polyhedra more \"regular\" would be to use an algorithm that evens up the edge lengths while leaving the vertices on the surface of the unit sphere. This could be done with a particle simulation similar to that I used for the [Hyperbolic Plane](%7Bfilename%7Dhyperbolic_tilings_processing.md) generation. A much simpler approach would be to implement a simple version of a repulsion algorithm, iterating over each point and only calculating $$1/r^2$$ repulsion forces for vertices connected by an edge.
