---
author: elfnor
category: think
date: '2015-05-29 22:00'
image: 'decimate\_planes\_31\_crop.png'
layout: post
tags:  blender
title: 'Blender Auto Save add-on'
---

(This image is unrelated to the post other than it was the 31st image rendered from the same basic geometry.)

My normal work flow in Blender is to create a blend file and use it to render and save an image often at low resolution and quality for speed. Then I fiddle some more (camera angle, lighting, materials), render another image hopefully save the Blender file and then repeat. I try to keep the same version and file names for images and blend files but I often get out of sync. What I need to be able to do is browse all the image versions and reliably find the blend file that produced it. That way I can go back and find the best images and render them again at a higher resolution and quality.

I first thought of a general solution using version control such as git. If I did a commit after each render I\'d have the kind of record I needed. But git dosen\'t store the image files in a way that the images are easily browseable between different commits.

I ended up making a small addition to an existing Blender add-on. The [original by Florian Meyer](http://wiki.blender.org/index.php/Extensions:2.6/Py/Scripts/Render/Auto_Save) auto saved the image when a render was completed.

My version available [here on github](https://github.com/elfnor/blender_auto_save_on_render), has the option to also auto save the blend file after each render. This results in nice pairs of blend files and images with the same name and version number.

After the add-on is installed, the settings are found on the Render panel.

![screenshot](%7B%7B%20site.baseurl%20%7D%7D/images/screen.png)

With \"Auto Save Image\" and \"with .blend\" set and the file \"\\Documents\\test.blend\" open, after rendering the following files will be created

``` {.python}
\Documents\auto_saves\test_001.png  
\Documents\auto_saves\test_001.blend  
```

If \"subfolder\" is also set the files will be created in a sub-folder named after the blend file.

``` {.python}
\Documents\auto_saves\test\test_001.png  
\Documents\auto_saves\test\test_001.blend  
```

The version number is incremented with each render.

This looks like it will do the job for me. I haven\'t yet done more than 999 versions of the same render.

I\'d still be interested in how other people handle version control stuff in Blender or other digital art.

Possibly most people have a more defined goal than me, know their tools well enough to head towards that goal in a linear fashion, know when they\'ve got there and then stop. Their last file is their best. Me, I tend to try lots of different ideas in different directions and stop when it\'s bedtime.

------------------------------------------------------------------------
