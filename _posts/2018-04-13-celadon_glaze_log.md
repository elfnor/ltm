---
author: elfnor
date: '2018-04-13 22:00'
layout: post
status: draft
tags:  blender
title: 'Example of a log file from Blender Auto Save Add-on'
---

This is an example of how the markdown log file produced by my latest version of the Blender [Auto Save on RenderAdd-on]({{ site.baseurl }}{% link _posts/2018-03-31-blender_auto_save_addon_update.md %}) can be annotated to keep a record of blender file changes and why they were made. Each time a blend file with the add-on enabled is rendered, the add-on saves a image and blend file copy with the same base file name in an `auto_saves` folder. The add-on also automatically adds the the filename of the blend and image pair, a datestamp, a markdown link to the image file and the render time to the log file.

As I worked on developing a celadon glaze material, I annotated this log file with what I was changing and what I thought of the result. I screen grabbed the node diagram when I changed its layout and added links to my reference image. This very easily gave me the record below of this little project to quickly develop a material for a celadon style glaze.

------------------------------------------------------------------------

Reference image\

Default material

**celadon\_glaze\_001** {2018-04-13 16:51}\
\
Render time: 0:00:20.183531

Add some color to diffuse

**celadon\_glaze\_002** {2018-04-13 16:57}\
\
Render time: 0:00:20.045838https://github.com/elfnor/blender\_auto\_save\_on\_render\
Too smooth but highlights are sharper

Roughen diffuse

**celadon\_glaze\_003** {2018-04-13 17:11}\
\
Render time: 0:00:20.356762

Try differnt glossy shader types

Sharp

**celadon\_glaze\_004** {2018-04-13 17:13}\
\
Render time: 0:00:20.082703\
Too glossy

Beckmann

**celadon\_glaze\_005** {2018-04-13 17:14}\
\
Render time: 0:00:20.434412\
Better but glossy highlight needs to be sharper

Lower glossy roughness

**celadon\_glaze\_006** {2018-04-13 17:17}\
\
Render time: 0:00:20.392772\
Better

Make mix use more of diffuse

**celadon\_glaze\_007** {2018-04-13 17:18}\
\
Render time: 0:00:20.221339\
Better

Try using Layer Weight for mix factor

**celadon\_glaze\_008** {2018-04-13 17:22}\
\
Render time: 0:00:20.346465

Facing instead of Fresnel

**celadon\_glaze\_009** {2018-04-13 17:24}\
\
Render time: 0:00:20.525302\
This is much darker around back edges

Lower Layer Weight Blend value

**celadon\_glaze\_010** {2018-04-13 17:26}\
\
Render time: 0:00:20.299810

Make both glossy and diffuse less rough

**celadon\_glaze\_011** {2018-04-13 17:28}\
\
Render time: 0:00:20.267659\
This has sharpened up highlight

Pick the colors off reference image

**celadon\_glaze\_012** {2018-04-13 17:34}\
\
Render time: 0:00:20.231184\
Not bad!

Final node setup

Could use a geometry pointedness input node to mix a lighter shade to give the look of a breaking glaze on sharp edges or play with a Voronoi texture for crazing.
