---
author: elfnor
category: think
date: '2018-03-31 22:00'
image: 'celadon\_glaze\_blend\_01\_027.png'
layout: post
tags:  blender
title: 'Update to Blender Auto Save add-on'
---

I \"version control\" my Blender work using my [add-on](http://elfnor.com/blender-auto-save-add-on.html) that auto saves a copy of the Blender file along with the rendered image, each time a render is completed. This allows me to work with low resolution (pixels or cycles) renders for speed. I can then go back and browse the images, and re-render the best images at higher resolution.

This works great, but as the files only have consecutive numbers there is no indication what was changed or why. What\'s missing in version control terms is both a \"commit\" message and the ability to see a \"diff\" between the Blender files.

Other people have thought about \"diff\" for Blender files. See [Get a diff between two .blend files - Blender Stack Exchange](https://blender.stackexchange.com/questions/7484/get-a-diff-between-two-blend-files), [CoDEmanX/blend\_stats](https://github.com/CoDEmanX/blend_stats), and [Blender python script to see changes in blend file - Blender Artists](https://blenderartists.org/forum/showthread.php?298560-Blender-python-script-to-see-changes-in-blend-files-%28patch%29) for other people thinking along these lines. As far as Google knows, no-one has implemented this.

I decided a much lighter solution was what I needed (and more achievable). Mostly, at the time I\'m working on a blend, I have some idea what settings I\'ve just changed and why. Capturing these in a \"commit\" message should be enough.

I\'ve updated my version of the [Auto Save on Render add-on](https://github.com/elfnor/blender_auto_save_on_render) to have an optional log file text block. This is in [markdown](https://daringfireball.net/projects/markdown) format and should be saved in the same folder as the auto-saved blend and image pairs. Each time the file is rendered, the add-on appends the filename of the blend and image pair, a datestamp, a markdown link to the image file and the render time to the log file. If the user keeps this file open in a Text Editor in Blender, they can very quickly add a comment to each render, covering what changes have been made and what the intent was.

If the markdown file is opened in an editor with in-line markdown images or markdown preview, the user can browse through the images, along with the comments. Extra comments can be added, at this point on the quality of the image, what still needs to be done, or anything else. This markdown file becomes a record of the project progress and could also be used for review by a second person.

    Default material  
    **celadon_glaze_001** {2018-04-13 16:51}
    ![](celadon_glaze_001.png)
    Render time: 0:00:20.183531

    Add some color to diffuse

    **celadon_glaze_002** {2018-04-13 16:57}
    ![](celadon_glaze_002.png)
    Render time: 0:00:20.045838
    Too smooth but highlights are sharper

    Roughen diffuse

    **celadon_glaze_003** {2018-04-13 17:11}
    ![](celadon_glaze_003.png)
    Render time: 0:00:20.356762

The add-on is intended to be unobtrusive and deliberately doesn\'t prompt the user for a comment or automatically save the text file externally to Blender.

![addon screen shot]({{ site.baseurl }}/images/auto_save_screenshot.png)

On the first render with the Auto Save on Render add-on enabled, and \"with log file\" checked, the add-on will add a text-data-block called \"save log\" to the Blender file. To see this after the first render, use the \"Browse text to be linked\" drop down below the text editor to make the \"save log\" the active text block. Add any comments desired. To save a copy externally to Blender use the \"Text \> Save As\" menu. The default save name has been set to \"blendfilebasename\_log.md\". This can be changed but the markdown file link format requires the log file be in the same folder as the auto-saved images.

I find this approach to version control is really suited to the texturing and lighting part of the development cycle when I\'m making lots of test renders. When I\'m modeling, I\'m now intending to set up a basic scene that I can periodically rendered just to use this style of version control. Of course large image sizes and large Blender files can quickly eat up hard drive space.

Here\'s an example of a the [log file](%7Bfilename%7Dceladon_glaze_log.md) used to develop the material used in the banner image. Its shows how the file with the addition of some simple annotations and screen shot clippings can become a good record of project progress or the start of a tutorial.
