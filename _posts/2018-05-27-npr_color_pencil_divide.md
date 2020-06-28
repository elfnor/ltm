---
author: elfnor
date: '2018-05-27 22:00'
image: 'fruits\_divide\_05.png'
layout: post
tags: blender npr compositing
title: 'NPR- color pencil using compositing nodes'
use_math: true
---

These are my experiments with getting a color pencil look using the compositing nodes in Blender.

The original blend file of the bowl of fruit comes from monomorph (David Bornemann) at [Fruits \| Blend Swap](https://www.blendswap.com/blends/view/44482).

The original has beautiful procedural cycles shaders with the skin texture given by voronoi displacements at a couple of scales.

![]({{ site.baseurl }}/images/fruits_bs_002.png)
*original render*

We\'re now going to mess with this in the compositor to get a color pencil look. This effect works very well with simple diffuse materials on the scene objects. It can also be used on photos as I\'ll show at the end of this post.

## Edges

In the [previous post]({{ site.baseurl }}{% link _posts/2018-05-12-npr_compositing_shadow.md %}) I used a freestyle render layer to give line effects. Here we\'ll produce them directly from the image. We want lines along the edges in the image, or put differently, in the parts of the image where the color or value of the image changes quickly.

There lots of ways to do this. Here we\'ll convert the image to values only, then divide the image by a blurred copy of itself. This works to as a high pass filter to detect the parts of the image where the value changes quickly.

In the parts of the image with little value change
$$
\text{blur}(image) \approx image
$$
therefore
$$
\frac{image}{\text{blur}(image)} \approx 1
$$
giving white.

Where the image value changes quickly
$$
\frac{image}{\text{blur}(image)} < 1
$$
giving dark pixels.

Here\'s a toy example. The top row of the table represents the image. The blur function in the second row just averages across the neighboring 3 pixels. After we divide the image by the blur and clamp the values to between \[0, 1\] we have a white image with a single dark pixel at the edge.

![]({{ site.baseurl }}/images/npr_color_pencil_divide-ebd5a.png)

[![node a]({{ site.baseurl }}/images/node_diagram_div_a_660.png)]({{ site.baseurl }}/images/node_diagram_div_a.png)
*nodes for isolating changes in value*{.caption}

![]({{ site.baseurl }}/images/fruits_dodge_02.png)
*result*

The width of the edge lines can be controlled by the size of the *Blur*. Also try different blur types for different effects.

The contrast in the edges can be strengthened with an *RGB Curves* node. I\'ve also added another *RGB Curves* before the edge nodes to give some control over how much value change produces an edge.

[![node a]({{ site.baseurl }}/images/node_diagram_div_b_660.png)]({{ site.baseurl }}/images/node_diagram_div_b.png)
*add contrast to the edges*{.caption}

![]({{ site.baseurl }}/images/fruits_dodge_03.png)
*result*

## Colors

Now we add some color back into the image,using the *Value* node. This takes the value from the edges, but the hue and saturation from the image. I\'ve also added a *Blur* to the color pass along with yet another *RGB Curves* for color adjustment.

[![node a]({{ site.baseurl }}/images/node_diagram_div_c_660.png)]({{ site.baseurl }}/images/node_diagram_div_c.png)
*adding color*{.caption}

![]({{ site.baseurl }}/images/fruits_dodge_04.png)
*result*

## Textures

The last thing to do is to add a texture to the whole image to simulate pencil strokes. This invloves combining a gray scale pencil stroke texture with the color image using the *Overlay* mode of the *Mix* node.

The overlay mode does different thing depending on whether the background(colored) image is light (\> 0.5) or dark (\< 0.5).

![]({{ site.baseurl }}/images/npr_color_pencil_divide-3d2b2.png)

This has the effect of overlaying the contrast of the greyscale texture on the colored image. The texture doesn\'t appear on the white or the black parts of the image.

Below is an example of a gray scale texture and some color swatches combined with the overlay mode. The circles are all 0.5 gray to show the color of the background square color swatches. Mouse over to show the gray scale texture.

<a ><img src="./images/overlay_example.png" onmouseover="this.src='./images/overlay_example_texture.png'" onmouseout="this.src='./images/overlay_example.png'" /></a>
*overlay with gray scale texture and color swatches*{.caption}

In the above image, the 9 color swatches on the left have at least one of their RGB channels greater than 0.5. For these colors the dark part of the texture takes the image toward the color defined by these channels. The 9 color swatches on the left have all their channels less than 0.5 and the dark part of the texture takes the image toward black.

Next, I\'ve used a paper texture taken from the GIMP fill dialog and mixed it with the color swatch image using the overlay mode. This works, a little, to give the effect of light and heavy pencil strokes, with a black pencil being used on the darker parts of the image.

![]({{ site.baseurl }}/images/paper_gimp.png)
*paper texture*

<a ><img src="./images/overlay_colors_paper.png" onmouseover="this.src='./images/overlay_paper.png'" onmouseout="this.src='./images/overlay_colors_paper.png'" /></a>
*overlay with gray scale paper texture and color swatches*{.caption}

The *Soft Light* mode would also be a good choice here. It doesn\'t have a sharp cutoff at 0.5 so gives an overall smother transition. The dark parts of the texture always produce a darker version of the image color.

<a ><img src="./images/soft_light_example.png" onmouseover="this.src='./images/overlay_colors_example.png'" onmouseout="this.src='./images/soft_light_example.png'" /></a>
*soft light with gray scale texture and color swatches*{.caption}

![]({{ site.baseurl }}/images/soft_light_paper.png)
*soft light with grey scale paper and color swatches*

Anyway, after the digression in to the *Overlay* mode, here is the node diagram used to combine the pencil texture with the color image to get our final result.

[![node a]({{ site.baseurl }}/images/node_diagram_div_d_660.png)]({{ site.baseurl }}/images/node_diagram_div_d.png)
*adding paper texture*{.caption}

<a ><img src="./images/fruits_divide_05.png" onmouseover="this.src='./images/fruits_bs_002.png'" onmouseout="this.src='./images/fruits_divide_05.png'" /></a>

A good result will require tweaking each of the four *RGB Curves* nodes. It worth while taking some time to understand how grading an image using curves works. Curves can be used to invert, posterize, gamma correct and more all with the same node. See the references below for more detail.

Many NPR techniques only seem to work well on the particular image or render given in the tutorial. A robust node setup should work over a range of images, with at most some tweaking of the *RGB Curves* for some fine tuning.

Here are a series of tests, with the same node setup used for the fruit bowl. Mouse over each to see the original.

<a ><img src="./images/sintel_pencil.png" onmouseover="this.src='./images/sintel_render.png'" onmouseout="this.src='./images/sintel_pencil.png'" /></a>

This render is of Sintel from the Blender movie via [Sintel Lite Cicles - Blender Guru \| Blend Swap](https:final//www.blendswap.com/blends/view/53062) and has similar settings as the fruit bowl. The next two are photographs and I tweaked the *RGB Curves* just a little bit to improve the final result.

<a ><img src="./images/feijoas_pencil.png" onmouseover="this.src='./images/feijoas_image.png'" onmouseout="this.src='./images/feijoas_pencil.png'" /></a>

<a ><img src="./images/tiger_pencil.png" onmouseover="this.src='./images/tiger_image.png'" onmouseout="this.src='./images/tiger_pencil.png'" /></a>

## References

Working well with compositing (rather than random fiddling, hoping for a good result) requires a good knowledge of how to use the *RGB Curves* node and the various modes of the *Mix* node. This knowledge can be gained by making your own experiments similar to my above color swatch tests and by reading other\'s explanations.

**RGB Curves**

[RGB Curves Node --- Blender Manual](https://docs.blender.org/manual/en/dev/render/blender_render/textures/nodes/types/color/rgb_curves.html) - especially the diagram that shows the curves for lighten, negative, decrease contrast and posterize.

[PIXLS.US - Basic Color Curves](https://pixls.us/articles/basic-color-curves/)\
[PIXLS.US - Color Curves Matching](https://pixls.us/articles/color-curves-matching/)

**Mix node modes**

[Blend modes - Wikipedia](https://en.wikipedia.org/wiki/Blend_modes)\
[Pixel Math \[Ebook\]](https://gumroad.com/l/jOAsw) - this gives the mathematics behind each of the mix modes as implemented in the Blender source code.\
[Clown Fish Cafe: GIMP\'s Layer Modes (Somewhat) Demystified](https://clownfishcafe.blogspot.com/2013/11/gimp-layer-modes-normal-to-addition.html)\
[El Brujo de la Tribu: Color Mix Modes in Blender / Cycles](https://elbrujodelatribu.blogspot.com/2013/02/color-mix-modes-in-blender-cycles.html)

------------------------------------------------------------------------
