---
author: elfnor
date: 2026-07-11 00:00
image:
  path: images/2026-07-11/blog_header-03-bright.png
layout: post
tags:
  - blender
title: Conway Polyhedra in Geometry Nodes
permalink: 2026-07-11-conway-polyhedra-in-geometry-nodes.html
---
[Blender 5.2](https://www.blender.org/) has a [Mesh Bevel Node](https://docs.blender.org/manual/en/5.2/modeling/geometry_nodes/mesh/operations/mesh_bevel.html)!!!

This inspired me to revisit my old work on [Conway Polyhedron Operators](https://elfnor.com/conway-polyhedron-operators-in-sverchok.html). I've  had a couple of (unpublished) attempts to implement these in Blender's Geometry Nodes, but was limited by the lack of a mesh bevel node (and my skill level). 

[Conway Polyhedra](https://en.wikipedia.org/wiki/Conway_polyhedron_notation) are formed by applying various operators to a seed polyhedron such as one of the platonic solids. Some of these are the same as nodes already available , for example, the _ortho_ operator is the same as the _Subdivide Mesh Node_ with _Level_ set to  1. Other operators are easily done in edit mode, for example, the _kis_ operator is the same as  _Poke Faces_ on the  Face menu. 

For an overview of the operators see [wikipedia](https://en.wikipedia.org/wiki/Conway_polyhedron_notation) To play online with the operators see either [polyhédronisme](https://levskaya.github.io/polyhedronisme/) or [George Hart](https://georgehart.com/conway/). I've implemented most of the Conway Operators in a [blend file](https://github.com/elfnor/blend_examples/tree/main) available on GitHub. All the useful node groups are marked as assets and will be available in the asset browser if you save the blend file  on an asset library path. ( for example  `~/Documents/Blender/Assets`)

In this post, rather than go into detail on every node group, I'm going to give an overview of the groups I've made. I hope to follow up with some  posts grouped on topic rather than node group. I have  a post  planned on evaluating attributes across domains, and another on debugging and testing strategies. 

## Conway assets

The blend file contains sixteen operators
![Conway Operator nodes](/images/2026-07-11/Pasted%20image%2020260709-115336.png)

There's also a bunch of seed polyhedra and seed tiles to combine with the operators.

![](/images/2026-07-11/Pasted%20image%2020260709-120256.png)

These are simple to construct  but its useful to have them easily accessible with a unit radius.

All the operators also work on 2D mesh tilings. There's a node group for triangles, squares and hexagons. These are approximately 2 m by 2 m and centred at the origin.

![](/images/2026-07-11/Pasted%20image%2020260709-120556.png)

Each of the above sets of nodes has an overall node with a drop down menu  to quickly switch between operators or between seeds. This is quicker for experimenting with combining operators than rewiring nodes. 

![](/images/2026-07-11/Pasted%20image%2020260709-121018.png)

To use these nodes combine a seed with a few operators. For example:

![](/images/2026-07-11/Pasted%20image%2020260709-152510.png)

If implementing a polyhedra in Conway notation, be aware that the notation and node order is reversed. That is, enter `wgD` on  [polyHédronisme](https://levskaya.github.io/polyhedronisme/) to get the polyhedron above.

Each of these subdivided polyhedra has a standard or canonical form. This form  has all  faces planar and all edges tangential to the unit sphere. The centre of the vertices of the polyhedra should also be at the origin. 

The _canon_ node does this. It has two inputs  _Iterations_  and _scale_. Applying it makes the polyhedron smoother and more symmetrical.

![](/images/2026-07-11/Pasted%20image%2020260709-154429.png)

Increase the  _Iterations_ ,  and maybe the  _scale_  until the polyhedron smooths out. The canon algorithm makes _scale_ sized improvement in _Iterations_ number of steps.  Too large a _scale_ value will blow the polyhedron up into a mess.

Here is a  gif of the each iteration of  the _canon_ node on `qoD` (a Dodecahedron with an _ortho_ operator followed by a _quinto_ operator) .

![](/images/2026-07-11/canon_slow_start.gif)

The canon operator is applied a few seconds into the animation. The sphere inflates quickly to near the unit sphere, than takes a  lot more iterations to  rotate the smaller quads until the edges connecting them become tangent to the unit sphere.

All the operator nodes work equally well on 2D tilings. Trying an operator on a 2D mesh often helps in understanding what it does

![](/images/2026-07-11/Pasted%20image%2020260709-154929.png)

This is  _gyro-whirl_ applied to the  _tri-tile_ grid.

Or _gyro-whirl_ on a [hyperbolic tiling](https://elfnor.com/2026-06-17-poincare-geometry-nodes-part-2.html)

![](/images/2026-07-11/Pasted%20image%2020260709-155458.png)


![](/images/2026-07-11/blog_show-off-polyhedra.png)

![](/images/2026-07-11/blog_show-off-tilings.png)