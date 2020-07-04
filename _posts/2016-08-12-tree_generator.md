---
author: elfnor
date: '2016-08-12 22:00'
image: 'sca-render_005.png'
layout: post
tags: ' blender sverchok generative-art'
title: Sverchok Tree Generator
---

I tend toward making more abstract than realistic images with Blender. A [while back]({{ site.baseurl }}{% link _posts/2015-05-17-ivy_generator.md %}) I played with the *Ivy Generator* addon and so got interested in the underlying procedural algorithms for producing tree and plant structures. A lot of these techiques use L-systems and could maybe be implemented using the Sverchok [Generative Art]({{ site.baseurl }}{% link _posts/2016-02-28-generative_art_docs.md %}) node. Except the *Genertive Art* node has no provision (yet) for collision avoidance. This results in intersecting branches which is not always what I want.

Somewhere in my research into procedural plants I came across the Space Colonization Algorithm which is a different approach to tree generation and decided to write this algoithm up as a Sverchok scripted node.

The Space Colonization Algorithim (SC Algorithm) was originally described in [this paper](http://www.academia.edu/download/46349919/colonization.egwnp2007.pdf) by Adam Runions, Brendan Lane and Przemyslaw Prusinkiewicz.

There are good short descriptions of how the algorithm works on [jgallant\'s blog](http://www.jgallant.com/procedurally-generating-trees-with-space-colonization-algorithm-in-xna/) or on [Jose\'s Sketchbook](Jose's%20Sketchbook).

> The idea for the **Space Colonization Algorithm** is brilliantly simple: instead of generating the nodes that build our structure by using a set of rules (e.g. a grammar), we begin by producing a set of **attractor points** by sampling a domain -the surface of an object in our case-. Then we provide one or several **start locations**, and we iteratively start adding nodes by looking for the set of **nearest *active* attractors**, and growing towards them. When a new node is added, it **kills all the attractor points** which fall within a specified range, preventing other branches to fill in the same space, and directing the growth forward.

Varkenvarken\'s Space Tree([free github version](https://github.com/varkenvarken/spacetree), [Space Tree Pro](https://cgcookiemarkets.com/all-products/space-tree-pro/)) addon for Blender uses the SC Algorithm to produce really versatile and realistic trees. My version is extremely simple but the ability to directly control the cloud of attractor points works well for my abstract doodles.

Here\'s a simple GIF showing the algorithm in action. The red points are the *End Vertices* or attractor points. There is one start point at (0, 0, 0).

![gif of sca]({{ site.baseurl }}/images/sca_gif.gif)

![node diagram for gif]({{ site.baseurl }}/images/sca_gif_nodes.png)

I started off looking at varkenvarken\'s github code but ended up completly rewriting it to use numpy. I then spent far longer than was reasonable optimizing my code to get a real-time response to parameter changes. The heart of the algorithm involves a large number of [nearest neighbor searches](https://en.wikipedia.org/wiki/Nearest_neighbor_search). This is a classic problem with many solutions (kdtrees, octrees, locality sensitive hashing). I looked at some of these but the fastest solution I could code (before I got bored) was to naively calculate every distance using numpy outer product. The whole SC Algorithm would make a great class (or competition) challenge in code speed optimization as there seems to be so many complicated ways to make it slower.

Install the [Sverchok](http://nikitron.cc.ua/sverchok_en.html) addon into [Blender](https://www.blender.org/). Download the Tree Generator code from [github](https://github.com/elfnor/spacetree-sverchok). Then load the python file *tree_generator.py* as a text blocks into a blend file. Add a *Scripted Node* to a Sverchok node tree. On the node select the *tree_generator.py* code from the lower drop down. Then click the plugin icon to the right of this field. The node should turn blue with some inputs and outputs.

The node inputs:

-   **maximum branches**: the number of iterations of the algorithm. The number of tree vertices will actually be greater than this  
-   **branch length**: distance beween tree vertices  
-   **minimum distance**: when a new tree vertex is added, all the attractor points within this radius are \"killed\"  
-   **maximum distance**: attractor points greater than this distance are ignored  
-   **tip radius**: used to calculate branch radii, the radius of the tip at the end of the branches  
-   **tropism**: vector that is added to each new branch segment, can be used to make branches droop under gravity. Length of vector needs to be smaller than branch length.  
-   **End Vertices**: set of attractor points that the tree will grow toward  
-   **Start Vertices**: start locations for tree to grow from

The node outputs:

-   **Vertices**: vertices (nodes) in tree structure  
-   **Edges**: edges conectiong vertices in tree structure  
-   **Branch radii**: radius of tree at each vertex. This is calculated using a \"pipe model\". That is at a vertex where two branches join, the area of the cross section is the sum of the areas of the two branches.  
-   **Ends mask**: a boolean list that is true for vertices that are at the end of a branch. Can be used with a *List Mask* node.  
-   **Leaf matrices**: a list of transform matrices that can be used to place an object at the end of each branch. The x-direction of the object will be aligned with the last segment in the branch.

Here is the Sverchok node diagram to produce the tree in the top image. A spherical cloud of atractor points is produced using the random vector and scalar nodes on the left. The *Skin Mesher* node is used to give thickness to the trunk and branches. The leaves are produced by the *Mesh Instancer* node. This make copies of a fairly simple leaf object (see insert) that has a image texture with an alpha channel. The textures are from EugeneKiver on [blendswap](http://www.blendswap.com/blends/view/59269). The modifiers on the tree trunk need to be applied before the UV texturing will work.

![sca tree nodes]({{ site.baseurl }}/images/sca_tree_nodes.png)

Here is another example (not so tree like) of what is possible with the Tree Generator.

![torus]({{ site.baseurl }}/images/sca_test3b_004.png)

I like to code my Blender procedural experiments ([maze any mesh]({{ site.baseurl }}{% link _posts/2016-01-29-maze_mesh.md %}), [2D mazes]({{ site.baseurl }}{% link _posts/2015-12-11-blender_maze_generator.md %}), [3D mazes]({{ site.baseurl }}{% link _posts/2015-12-20-blender_3D_maze_generator.md %}), [hyperbolic plane tilings]({{ site.baseurl }}{% link _posts/2015-11-06-hyperbolic_tilings.md %}), [more hyperbolic planes]({{ site.baseurl }}{% link _posts/2015-04-15-hyperbolic_planes.md %}), [bloom spheres]({{ site.baseurl }}{% link _posts/2015-09-09-bloom_spheres.md %})) as Sverchok nodes as I can concentrate on getting my code to produce simple lists (of vertices, edges, faces, and matrices) and Sverchok takes care of produing the actual mesh in Blender. The inputs and parameters provide a simple interface for the code without messing with the Blender API.

------------------------------------------------------------------------
