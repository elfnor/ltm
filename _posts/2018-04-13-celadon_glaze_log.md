---
author: elfnor
date: '2018-04-13 22:00'
layout: post
status: draft
tags:  blender
title: 'Example of a log file from Blender Auto Save Add-on'
permalink: /drafts/example-of-a-log-file-from-blender-auto-save-add-on.html
---

This is an example of how the markdown log file produced by my latest version of the Blender [Auto Save on RenderAdd-on]({{ site.baseurl }}{% link _posts/2018-03-31-blender_auto_save_addon_update.md %}) can be annotated to keep a record of blender file changes and why they were made. Each time a blend file with the add-on enabled is rendered, the add-on saves a image and blend file copy with the same base file name in an `auto_saves` folder. The add-on also automatically adds the the filename of the blend and image pair, a datestamp, a markdown link to the image file and the render time to the log file.

As I worked on developing a celadon glaze material, I annotated this log file with what I was changing and what I thought of the result. I screen grabbed the node diagram when I changed its layout and added links to my reference image. This very easily gave me the record below of this little project to quickly develop a material for a celadon style glaze.

------------------------------------------------------------------------

Reference image  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_vase_03.jpeg)

Default material    

**celadon_glaze_001** {2018-04-13 16:51}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_001.png)  
Render time: 0:00:20.183531  

![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_ss_01.png)

Add some color to diffuse  

**celadon_glaze_002** {2018-04-13 16:57}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_002.png)  
Render time: 0:00:20.045838https://github.com/elfnor/blender_auto_save_on_render  
Too smooth but highlights are sharper    

Roughen diffuse  

**celadon_glaze_003** {2018-04-13 17:11}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_003.png)  
Render time: 0:00:20.356762   

Try differnt glossy shader types  

Sharp  

**celadon_glaze_004** {2018-04-13 17:13}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_004.png)  
Render time: 0:00:20.082703  
Too glossy  

Beckmann  

**celadon_glaze_005** {2018-04-13 17:14}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_005.png)  
Render time: 0:00:20.434412  
Better but glossy highlight needs to be sharper  

Lower glossy roughness  

**celadon_glaze_006** {2018-04-13 17:17}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_006.png)  
Render time: 0:00:20.392772   
Better  

Make mix use more of diffuse  

**celadon_glaze_007** {2018-04-13 17:18}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_007.png)  
Render time: 0:00:20.221339  
Better  

Try using Layer Weight for mix factor  

**celadon_glaze_008** {2018-04-13 17:22}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_008.png)  
Render time: 0:00:20.346465  

![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_ss_02.png)  

Facing instead of Fresnel

**celadon_glaze_009** {2018-04-13 17:24}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_009.png)  
Render time: 0:00:20.525302  
This is much darker around back edges

Lower Layer Weight Blend value

**celadon_glaze_010** {2018-04-13 17:26}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_010.png)  
Render time: 0:00:20.299810  

Make both glossy and diffuse less rough

**celadon_glaze_011** {2018-04-13 17:28}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_011.png)  
Render time: 0:00:20.267659  
This has sharpened up highlight

Pick the colors off reference image

**celadon_glaze_012** {2018-04-13 17:34}  
![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_012.png)  
Render time: 0:00:20.231184   
Not bad!

![]({{ site.baseurl }}/images/celadon_glaze/celadon_vase_03.jpeg)  

Final node setup

![]({{ site.baseurl }}/images/celadon_glaze/celadon_glaze_ss_03.png)  

Could use a geometry pointedness input node to mix a lighter shade to give the look of a breaking glaze on sharp edges or play with a Voronoi texture for crazing.
