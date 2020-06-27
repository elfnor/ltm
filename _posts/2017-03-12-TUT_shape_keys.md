---
author: elfnor
category: think
date: '2017-03-12 22:00'
image: 'TUT\_shape\_keys\_00.png'
layout: post
tags:  blender
title: Blender Shape Keys Cookbook
---

(**1:** Sphere with both shape keys Value = 0. **2-3:** Two different shape keys *Value* = 1. **4:** A mix of the two previous shape keys)

The animation in [Shape Shifting Creatures](%7Bfilename%7D) was the first time I\'d used shape keys in Blender. I only decided to animate between the different animal shapes after I modeled both shapes on different meshes. Then I learnt that shape keys were the way to animate a transition between two mesh shapes. Getting from two meshes to one mesh with shape keys (then add a third mesh, sort out the mesh messes I made..) meant I needed to copy meshes to shape keys, apply shape keys and other things I could have avoided if I\'d known what I was doing before I started. But you never learn anything (or go anywhere really interesting) if you know where you\'re going before you start out.

All this copying, applying and swapping shape keys is possible in Blender but not always obvious. Here is a little cookbook of how do some of these useful things with shape keys. Thanks to [Blender artists](https://blenderartists.org/forum/) and [Blender stack exchange](http://blender.stackexchange.com/) for the answers (and interesting places to go).

Just a couple of points:

-   If you add or subtract vertices while editing meshes with shape keys strange things can sometimes happen. Vertices added while editing the *Basis* key in areas not affected by other shape keys mostly behave as expected but in general it\'s best (for beginners anyway and definitely on complicated meshes) to not add or delete vertices after adding shape keys. The techniques below only work if the two meshes involved have the same number of vertices connected to each other in the same way.
-   Modifiers such as *Mirror*, *Subdivision Surface*, *Boolean* can\'t be applied to a object that has shape keys.
-   Most examples of using shape keys are for adding small deformations on a large mesh, such as animating facial expressions or mouth shapes for speech. Shape keys aren\'t limited to this and can be used for large mesh deformations. One limitation is that the *Value* slider will interpolate a vertex position along a straight line between the two extreme positions of a vertex. If you need to move a vertex in a curve you need to either use many shape keys to approximate a curve or a different animating technique.

To start off learning about shape keys [RTFM](https://docs.blender.org/manual/en/dev/animation/shape_keys/index.html).

**Table of Contents**

\[TOC\]

## Copy a mesh to a shape key

**Problem**

You already have two meshes with the same number and order of vertices but different shapes (vertex positions). You want to smoothly transition between the two for an animation.

**Solution: Join as Shapes**

Normally you would apply shape keys to an object with the first mesh shape and then edit this object to give the second mesh shape. Both mesh shapes are stored on the same object as shape keys. In this problem we have two separate objects and want to copy the mesh shape from the second object so it becomes a shape key on the first object. This could be because the meshes were produced by a python script or imported from another program.

Here are two meshes:

![two meshes]({{ site.baseurl }}/images/TUT_shape_keys_01.png)

Add a \"Basis\" *Shape Key* to the left object. The *Shape Key* panel is found in the *Properties Editor* under the *Object Data* tab.

![adding shape key]({{ site.baseurl }}/images/TUT_shape_keys_02a.png)

Select the right object then `Shift-RMB`{: .kbd} the left object. Both objects are selected and the left object is the active object.

Click the small dark triangle on the *Shape Key* panel and choose the *Join as Shapes* option. This will add a shape key with the name of the right object.

![join as shapes]({{ site.baseurl }}/images/TUT_shape_keys_03.png)

If you select this *Shape Key* and drag the slider below the left object will transition from its original shape to the shape of the right object.

![transition]({{ site.baseurl }}/images/TUT_shape_keys_04.png)

The position of the slider can be added as a keyframe in an animation by either hovering over the slider and pressing `I`{: .kbd} or with the context menu by `RMB`{:.kbd} a property and choose *Insert Keyframe* from the menu.

## Replace the basis mesh of an object with a different mesh shape

**Problem:**

You have an object with a base mesh and a shape key that changes the shape of the base mesh. You\'d like to change the base mesh shape. You can just select the *Basis* Key under *Shape Keys* and edit the mesh to change the base shape. In this problem, the shape we want the base mesh to have already exists on a different object. We need to copy the shape key from the original base mesh to our new base mesh.

**Solution: Transfer Shape Key**

The left object is the original object and has a shape key. The right object is the new base mesh. Delete all shape keys from the right object.

![two more meshes]({{ site.baseurl }}/images/TUT_shape_keys_05.png)

Select the left object then `Shift-RMB`{: .kbd} the right object. Both objects are selected and the *right object* is the active object.

Click the small dark triangle on the *Shape Key* panel and choose the *Transfer Shape Key* option. This will copy the shape key from the left object to the right object.

![transfer shape key]({{ site.baseurl }}/images/TUT_shape_keys_06.png)

The right object now has the new base mesh and the old shape key.

## Apply a shape key to a mesh

**Problem:**

You want to fix the mesh shape at a particular value of a shape key.

**Solution: New Shape From Mix**

Set the *Shape Key* *Value* slider to give the desired mesh shape. If you have multiple shape keys set all the sliders as needed.

Click the small dark triangle on the *Shape Key* panel and choose the *New Shape From Mix* option. This will add a new shape key from the current mix of values.

![new shape from mix]({{ site.baseurl }}/images/TUT_shape_keys_07.png)

Delete all the shape keys making sure to delete the new key last.

## Swap a shape key with the *Basis* key

**Problem:**

Moving the *Value* slider on your shape key changes the object shape from the basis shape (shape A) when the value is 0 to the key shape (shape B) when the slider is 1. You want this to work the other way, shape B has a value of 0 and shape A has a value of 1.

**Solution: Move the Shape Key**

You could do this by duplicating the object, applying the shape keys (at the correct slider value) to give one mesh with shape A and one mesh with shape B. Then copying the shape A mesh to a shape key on the mesh with shape B. But there\'s also a more straightforward way.

Use the arrows on the side of the *Shape Key* panel to move the shape key to the top of the list.

![move shape key]({{ site.baseurl }}/images/TUT_shape_keys_08.png)

The slider no longer moves between shapes. With the *Basis* key (now at the bottom of the list), click the push pin underneath the key list. Shape A nows shows

![push pin]({{ site.baseurl }}/images/TUT_shape_keys_09.png)

Click the small dark triangle on the *Shape Key* panel and choose the *New Shape From Mix* option. This will add a new shape key that has the visible shape (Shape A). Click the push pin again to deselect it. The slider on the second key should now move between shape B at a value of 0 to shape A at a value of 1.

![new key]({{ site.baseurl }}/images/TUT_shape_keys_10.png)

Delete the original *Basis* key. Rename the shape key at the top of the list to *Basis*. This is optional but it might avoid you confusing yourself later (I\'m very good at doing stuff that confuses me later).

![rename key]({{ site.baseurl }}/images/TUT_shape_keys_11.png)

------------------------------------------------------------------------
