---
author: elfnor
date: '2014-07-18 22:00'
image: 'morenaments.png'
layout: post
slug: Symmetry Group Links and Notation
summary: 'There are many great resources on the 17 plane symmetry groups. I made great use of a couple of on-line apps while writing my \[Symmetry Tile plug-in.\]({filename}/symmetry\_tile\_docs.md), Morenaments and Kalis. I also used a couple of great books\...'
tags: ' symmetry-tile'
title: Symmetry Group Links and Notation
---

There are many great resources on the 17 plane symmetry groups.

I made great use of a couple of on-line apps while writing my [Symmetry Tile plug-in.]({{ site.baseurl }}{% link _posts/2014-07-05-symmetry_tile_docs.md %})

## Morenaments

This great Java applet can either be used [here on-line](http://www.morenaments.de/euc/applet) or [downloaded](http://www.morenaments.de/euc/) as a `jar` file and run locally.

It allows you to draw on a canvas that automatically completes the chosen symmetry group.Be sure to investigate the menu on the top-right. Selecting tile and/or cell under the grid menu will show these on the pattern canvas and help with seeing the structure underlying a particular symmetry. Look in the manual under help for more information. Be sure to try dragging the coloured dots in the tile view around. These can show how the same symmetry pattern can have different shaped cell or tile.

## Kali

![kali]({{ site.baseurl }}/images/kali.png)

This on-line Java applet can be used [here](http://www.scienceu.com/geometry/handson/kali/). Its much easier to do straight lines in this app. It uses the orbifold notation for the symmetry groups.

## Books

The two books I consulted the most while working on this plug-in were:

-   [\"Handbook of Regular Patterns: An Introduction to Symmetry in Two Dimensions\" by Peter S. Stevens](http://www.amazon.com/Handbook-Regular-Patterns-Introduction-Dimensions/dp/0262690888)  
-   [\"Designing Tessellations: The Secret of Interlocking Patterns\" by Jinny Beyer](http://www.amazon.com/Designing-Tessellations-Secrets-Interlocking-Patterns/dp/0809228661/)

All of these references use different notations and descriptions for the symmetry groups. I\'ve summarised them in the following table for easy reference. The Symmetry Tile plug-in uses the notation in the left most column.

## Notation for Symmetry Groups

  ---------------------------------------------------------------------------------------------------------------------------------------------------
  Crystallography   full   Terrazo         Jinny Beyer\'s description          Orbifold   Peter S. Stevens\'s description
  ----------------- ------ --------------- ----------------------------------- ---------- -----------------------------------------------------------
  p1                p1     Gold Brick      Translation                         o          Two Nonparallel Translations

  p2                p211   Hither & Yon    Midpoint or Half-Turn Rotation      2222       Four Half-Turns

  pm                p1m1   Wings           Mirror                              \*\*       Two Parallel Mirrors

  pg                p1g1   Card Tricks     Glide                               xx         Two Parallel Glide Reflections

  pgg               p2gg   Honey Bees      Double Glide                        22x        Two Perpendicular Glide Reflections

  pmm               p2mm   Prickly Pear    Double Mirror                       \*2222     Reflections in Four Sides of a Rectangle

  pmg               p2mg   Lightning       Glided Staggered Mirror             22\*       A Mirror and a Perpendicular Reflection

  cm                c1m1   Crab Claws      Staggered Mirror                    \*x        A Reflection and a Parallel Glide Reflection

  cmm               c2mm   Spider Web      Staggered Double Mirror             2\*22      Perpendicular Mirrors and Perpendicular Glide Reflections

  p4                p4gm   Pinwheel        Pinwheel or Quarter-Turn Rotation   442        Quarter-Turns

  p3m1              p3m1   Winding Ways    Mirror and Three Rotations          \*333      Reflections in an Equilateral Triangle

  p3                p3     Storm at Sea    Three Rotation                      333        Three Rotations through 120°

  p4g               p4gm   Primrose Path   Mirrored Pinwheel                   4\*2       Reflections of Quarter-Turns

  p4m               p4mm   Sunflower       Traditional Block                   \*442      Reflections on the Sides of a 45°-45°-90° Triangle

  p6                p6     Whirlpool       Six Rotation                        632        Sixfold Rotation

  p31m              p31m   Monkey Wrench   Three Rotations and a Mirror        3\*3       Refections of 120° Turns

  p6m               p6mm   Turnstile       Kaleidoscope                        \*632      Refections in the Sides of a 30°-60°-90° Triangle
  ---------------------------------------------------------------------------------------------------------------------------------------------------

The symmetry groups that can be made with rectangular or square cells can be defined using the [\"bdpq\" notation]({{ site.baseurl }}{% link _posts/2014-07-05-symmetry_tile_docs.md %}). If the \"bdpq\" string contains a plus sign then the cell must be square. The strings for all the symmetry groups start with a \"b\". Each of the symmetry groups could be created with an alternative string starting with one of the other letters.

The 32 strings here are this produced by the Symmetry Tile plugin when \"all square cells\" is selected and \"Multiple\" is set to \"Yes\". When \"Multiple\" is set to \"No\" only one string from each symmetry group is used.

<table>
  <tr>
    <th>Symmetry Group</th>
    <th>bdpq string</th>
  </tr>
  <tr>
    <td>p1</td>
    <td>b</td>
  </tr>
  <tr>
    <td>p2</td>
    <td>bq</td>
  </tr>
  <tr>
    <td></td>
    <td>b|q</td>
  </tr>
  <tr>
    <td></td>
    <td>bq|qb</td>
  </tr>
  <tr>
    <td>pm</td>
    <td>bd</td>
  </tr>
  <tr>
    <td></td>
    <td>b|p</td>
  </tr>
  <tr>
    <td>cm</td>
    <td>bp|pb</td>
  </tr>
  <tr>
    <td></td>
    <td>bd|db</td>
  </tr>
  <tr>
    <td>cmm</td>
    <td>bdpq|pqbd</td>
  </tr>
  <tr>
    <td></td>
    <td>bd|qp|db|pq</td>
  </tr>
  <tr>
    <td></td>
    <td>bqpd|pdbq</td>
  </tr>
  <tr>
    <td></td>
    <td>bd|pq|db|qp</td>
  </tr>
  <tr>
    <td>pg</td>
    <td>bp</td>
  </tr>
  <tr>
    <td></td>
    <td>b|d</td>
  </tr>
  <tr>
    <td></td>
    <td>bd+|d+b</td>
  </tr>
  <tr>
    <td></td>
    <td>bp+|p+b</td>
  </tr>
  <tr>
    <td>pgg</td>
    <td>bp|dq</td>
  </tr>
  <tr>
    <td></td>
    <td>bq|dp</td>
  </tr>
  <tr>
    <td></td>
    <td>bp|qd</td>
  </tr>
  <tr>
    <td>pmg</td>
    <td>bd|qp</td>
  </tr>
  <tr>
    <td></td>
    <td>b|p|d|q</td>
  </tr>
  <tr>
    <td></td>
    <td>b|q|d|p</td>
  </tr>
  <tr>
    <td></td>
    <td>bdpq</td>
  </tr>
  <tr>
    <td></td>
    <td>bqpd</td>
  </tr>
  <tr>
    <td></td>
    <td>bq|pd</td>
  </tr>
  <tr>
    <td>pmm</td>
    <td>bd|pq</td>
  </tr>
  <tr>
    <td>p4</td>
    <td>bb+|q+q</td>
  </tr>
  <tr>
    <td></td>
    <td>bq+|b+q</td>
  </tr>
  <tr>
    <td>p4g</td>
    <td>bdp+b+|pqq+d+|p+b+bd|q+d+pq</td>
  </tr>
  <tr>
    <td></td>
    <td>bdd+q+|b+p+pq|d+q+bd|pqb+p+</td>
  </tr>
  <tr>
    <td></td>
    <td>bb+p+d|q+qpd+|p+dbb+|pd+q+q</td>
  </tr>
  <tr>
    <td></td>
    <td>bq+d+d|pp+b+q|d+dbq+|b+qpp+</td>
  </tr>
</table>

------------------------------------------------------------------------
