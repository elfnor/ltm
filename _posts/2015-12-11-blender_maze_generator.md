---
author: elfnor
date: '2015-12-11 22:00'
image: 'maze_2d_steey_taws_01.png'
layout: post
tags:  blender sverchok maze
title: Blender 2D Maze Generator
permalink: blender-2d-maze-generator.html
---

The [game levels]({{ site.baseurl }}{% link _posts/2015-06-11-blender_game_level_generator.md %}) that can be produced by the *Generative Art* Sverchok node in Blender are lots of fun to explore but there\'s nothing to stop them producing overlapping paths like this:

![overlaps in Steely Taws]({{ site.baseurl }}/images/level_gen_STD_ramps_render_015.png)

This is because the alogrithim behind the node is based on L-systems and knows nothing about where its already been. The code only keeps tracks of a current transform (encoding position and direction) and randomly chooses another transfom rule to apply. It has no way to know where anything has already been drawn.

The game levels from the *Generative Art* node can be edited to produce a playable layout. See this [great puzzle game](http://techmonkeybusiness.com/steely-taws-puzzle-game-v3.html) complete with a Gondola, from Hamish at TechMonkeyBusiness. But I wanted to produce some more orderly game levels so I started looking at maze generating algorithms. I found [Jamis Buck\'s](http://weblog.jamisbuck.org/2011/2/7/maze-generation-algorithm-recap) great posts on maze building and his book \"Mazes for Programmers\".

In my ususal magpie fashion I started off with someone else\'s bright shiny code, in this case [this gist](https://gist.github.com/samisalkosuo/77bd95f605fc41dc7366) by Sami Salkosuo, which is a python version of some of the code from \"Mazes for Programmers\".

This implements the recursive backtracker algoritm as described on Buckblog. Imagine a grid of squares for a a 2D maze. The squares represent cells that will become paths in the maze. The lines at the edges of the squares are \"carved\" through to connect the cells to form a maze path.

> Here's the mile-high view of recursive backtracking

> Choose a starting point in the field.  
> Randomly choose a wall at that point and carve a passage through to the adjacent cell, but only if the adjacent cell has not been visited yet. This becomes the new current cell.
> If all adjacent cells have been visited, back up to the last cell that has uncarved walls and repeat.
> The algorithm ends when the process has backed all the way up to the starting point.

Theres\'s an animation of this [here](http://weblog.jamisbuck.org/2011/2/7/maze-generation-algorithm-recap).

The algorithm can either be implemented using recursive function calls, or with a stack that holds the cells visited, when a passage is carved through to another cell the new cell is added to the end of the stack. When the algorihtm backtracks cells are poped off the end of the stack.

I\'ve included a version of this code in a Sverchok scripted node (*maze_2d_gen*) that like the *Generative Art* node has a matrices and a mask outputs. The matrices list is used to position objects and the mask list of integers is used to determine which object to place at each position. To use this you\'ll need both the the `maze_3d.py` and the `maze_2D_gen.py` files from [github](https://github.com/elfnor/mazes).

I\'ll start with a simple example `maze_2D_simple.blend`that just has very basic components and show how to generate either a path maze or a wall maze. Then I\'ll show you how to make a 2D maze with some of the Steely Taws game components. Then I\'ll finish up with the blend file with the components, Sverchok node diagram and the game logic to allow anyone to generate and play an unlimited number of random Steely Taws game mazes. These mazes are all just 2D but the 3D code in `maze_3d.py` shows what I\'m working on next.

For a demo version to show how this works I made simple straight, bend, t-intersection and 4 way cross intersection tiles. Each tile needs to fit in to a 1 unit by 1 unit square centered on the origin. The straight section goes from (-0.5, 0, 0) to (0.5, 0, 0). The bend goes from (1, 0, 0) to (0, 1, 0) and has its origin at (0, 0, 0). The t-intersection is the same length as the straight but has a path going to (0, 1, 0). In the `maze_2D_simple.blend` file these components are on the second layer.

![simple game components]({{ site.baseurl }}/images/maze_2d_pieces.png)

The Sverchok node diagram is very similar to those used with the *Generaive Art* node. A *Logic* node and a *Mask* node are used to separate the matrices output into four lists for the four different types of tiles (straights, bends, tees and crosses) that need to be placed.

[![node diagram]({{ site.baseurl }}/images/maze_2d_nodes_crop.png)]({{ site.baseurl }}/images/maze_2d_nodes_full.png)

![simple level]({{ site.baseurl }}/images/maze_2d.png)

The size of the maze can be set with the *height* and *width* sliders on the node. Changing the *rseed* slider generates a new random maze. The *scale* sets the distance between components.

A braided maze is a maze that has loops. If *braid* is set to zero there will be no loops and there will be only one path between any two parts of the maze. This type of maze has lots of dead ends (cells with only one connecting path). If *braid* is set to 1 there will be no dead ends and lots of loops. In between, *braid* sets the proportion of dead ends that are turned into loops.

![simple level]({{ site.baseurl }}/images/maze_2d_braid.png)

![rendered level]({{ site.baseurl }}/images/maze__2D_simple_042.png)

For some mazes there may be no 4 way intersections (or for braid = 1, no ends), in this case you need to turn the display of ths object off or you\'ll get an out of place cross (or end) at the origin.

With a different set of tiles its easy to produce a typical maze with walls between the passages. The `maze_2D_simple.blend` file has some tiles for this on the third layer. Swap them for the path tiles gives a maze like this.

![tiles for walled maze]({{ site.baseurl }}/images/maze_2d_walls.png)

![walled maze]({{ site.baseurl }}/images/maze__2D_simple_02_012.png)

Try hedges or a layout such as a garden that is less obviously a maze.

The `steely_taws_2D_maze.blend` file has all the parts to generate and play an unlimited number of marble run mazes.

Install a recent version of [Sverchok](https://github.com/nortikin/sverchok) then open the blend file. A simple maze with 100 tiles should be automatically generated. Maximise the 3D View (CTRL-UP). Select the camera view (NUMPAD-0) then press P to start the game. Controls are provided for either the arrow keys or a joystick. If you fall off the edge you will automatically respawn at the start.

To make your own levels, press ESC to close the game and return to Blender. Change the the \"rseed\" value in the node and get a new game.

In the blend file I\'ve used \"mesh instancer\" nodes to the replace the \"Objects In\" and \"Viewer Draw\" nodes. This allows the same mesh to be used for every straight in a similar way to when you do a linked duplicate in Blender and produces a much smaller file.

Here\'s another screen shot of the game being played in the Blender Game Engine.

![screen shot 1 of game in play]({{ site.baseurl }}/images/maze_2d_steely_taws_02.png)

------------------------------------------------------------------------
