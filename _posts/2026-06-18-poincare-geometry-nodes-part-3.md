---
author: elfnor
date: 2026-06-18 00:00
image:
  path: images/2026-06-18/header-poincare-part-3.png
layout: post
tags:
  - blender
title: "Patterns in the Poincaré Plane - Geometry Nodes: Part 3"
permalink: 2026-06-18-poincare-geometry-nodes-part-3.html
---


This is the third part of a series on using Blender Geometry nodes to make some pretty patterns on the Poincare disk . Part1, Part2. 

So far I made a tiling of the Poincare Disk, but for simplicity I drew  the tiling edges with straight lines.  The edges in the tilings should really be arcs because in Poincare geometry the shortest distance between two points is a geodesics or circle arc.

I found it  easiest to replace the edges after constructing the tiling. I used two nested "For Element " loops. The outer one iterates over every face in the tiling, the inner one over every edge in the face. This duplicates calculations but it's fast enough, so no optimisation needed.

Inner loop

![HypSegments inner loop nodes](/images/2026-06-18/Pasted%20image%2020260603171356.png)
The inner loop is simple and just creates a `HypSegment` through the two points of each edge on the face.

Outer  Loop

![outer loop HypSegments nodes](/images/2026-06-18/Pasted%20image%2020260603171543.png)


The outer loop (optionally) fills the curve with an n-gon. The `Curve to Mesh` ` Merge by Distance` `Mesh to Curve` node sequence, merges the extra vertices at the face corners and sorts the vertices into sequential order. After that we replace the curves with a filled mesh circle with the appropriate number of vertices, then set the vertex position to that sampled from the curve.

![curved edge tiling](/images/2026-06-18/Pasted%20image%2020260612155417.png)

The full node group `p-tiling-arc-option` in the available blend file. has some other draw options.

![tiling group options](/images/2026-06-18/Pasted%20image%2020260609204134.png)


`geodesics` draws a full  `HypLine` between the ideal points on the edge of the Poincare Disk (the unit circle) for every edge.

![geodesics option](/_site/images/2026-06-18/Pasted%20image%2020260612160107.png)

The off centre options allow the polygon that starts the tiling to be displaced from the origin. Setting this up involved creating some more basic tools for working with hyperbolic geometry.
- `h-distance`- the length of the geodesic between two points
-  h-circle - the euclidean centre and radius of a hyperbolic circle.

### `h-distance`

The hyperbolic distance  is the shortest distance between two points in the Poincare Disk.  A derivation is given here - [GCT Measurement in Hyperbolic Geometry](https://mphitchman.com/geometry/section5-3.html)  that doesn't require finding the ideal points of the geodesic.  The result there is given in complex number notation.

$$
d_H(p, q) = |\ln{(\frac{|1-\overline{p}q|+|q-p|}{|1-\overline{p}q|-|q-p|})}|
$$
where $p =p_x +ip_y$ and $q= q_x +iq_y$ are complex numbers and  $\overline{p} =p_x -ip_y$ is the complex conjugate of $p$
swapping to vector notation $P=(p_x, p_y)$  and $Q=(q_x, q_y)$,

$$
\overline{p}q = (P\cdot Q, |P \times Q|)
$$
$P\cdot Q$ is the dot product and $|P\times Q|$ is the length of the cross product.
 or in math formula extension notation
 
```js
ng h_distance(P: vec3, Q:vec3) -> hd: float{
	qp = dist(P,Q);
	ccpq = length({1 - dot(P, Q),length(cross(P, Q)),0});
	out hd = abs(log((ccpq + qp)/(ccpq - qp), #e));
}
h_distance({0.5, 0, 0}, {0, 0.5, 0});
```

![h-distance node group](/images/2026-06-18/Pasted%20image%2020260604171939.png)

### `h-circle`

For a hyperbolic circle all the points are an equal hyperbolic distance from the hyperbolic centre.  It can be drawn as an Euclidean circle with the Euclidean centre $B_E$ offset from the hyperbolic centre $B$ on a line toward the origin $O$. 

![h-circle geogebra](/images/2026-06-18/Pasted%20image%2020260608173900.png)

Distances from a point to the the origin of the Poincare Disk can be converted back and forth from  hyperbolic $d_H$ to Euclidean $d_E$  distances via

$$
d_H=|ln(\frac{1+d_E}{1-d_E})|
$$
$$
d_H = 2 \tanh^{-1} d_E
$$

and
$$
d_E = \tanh(\frac{d_H}{2})
$$

where $\tanh$ is the hyperbolic tangent function.

Using this we can find he Euclidean centre $B_E$ of a hyperbolic circle, centre at $B$, through $A$


$$
r_H
= d_H(\overline{A},\overline{B})
$$
$$
b_H = d_H(\overline{B},\overline{O})
$$

$$
e1= \tanh(\frac{b_H+r_H}{2})
$$

$$
e2= \tanh(\frac{b_H-r_H}{2})
$$

$$
B_E = \frac{e1+e2}{2}\frac{\overline{B}}{|B|}
$$


![h-circle](/images/2026-06-18/Pasted%20image%2020260608175401.png)




Now I have the `h-circle` group, I can use it to find the perpendicular bisector geodesic of the desired  centre of a hyperbolic polygon and the origin.  then I reflect a polygon at the origin through this geodesic to place the vertices of the off centre polygon.

The hyperbolic perpendicular bisector construction is analogous to the familiar Euclidean one.  Draw a circle centred on A through B, and another circle centred on B through A. Find the two intersections points of these circles. Draw a line though them.  

![perpendicular bisector geogebra](/images/2026-06-18/Pasted%20image%2020260608194037.png)


![perpendicular bisector nodes](/images/2026-06-18/Pasted%20image%2020260608193937.png)

And the full  `off-center-polygon` group

![off center polygon node group](/images/2026-06-18/Pasted%20image%2020260608194139.png)


This is incorporated into the `p-tiling` group,within the `p-tiling-arc-option` to produce Poincare tilings with an off centre polygon.

![vertex centered tiling](/images/2026-06-18/Pasted%20image%2020260612160039.png)


The `off-center-polygon`  can also be used to draw other patterns.

Here a hyperbolic line is drawn through the centre and each vertex of a polygon.

![polygon radials](/images/2026-06-18/Pasted%20image%2020260608201906.png)

So that wraps this series for now. Although I have some ideas for hyperbolic solids in 3D ...

All the hyperbolic tools can be found as assets in the provided blend file. Most of the pretty pictures are available in the "demos" collection in that file. There's also a "tests" collection, that has tests for the `h-circle` and `h-distance` groups. I needed to rewrite these two groups as I refined and simplified the maths. I wanted to find a way to use tests in a similar way that I would use with text code. 

Overall, this was a great learning exercise. I've improved my ability to implement maths and geometry in Blender Geometry Nodes heaps. I feel I've got my fingers over the edge of the learning cliff and can now pull myself up. Hopefully, I'll document some more of this here on the blog.








