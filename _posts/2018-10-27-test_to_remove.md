---
author: elfnor
date: '2018-10-27 22:00'
image: composite_540_full_focus_05.png
layout: post
status: published
tags:  deploy
title: 'Examples of stuff to test deployment'
use_math: true
---

# Images

This is an image  using 
`{{ site.baseurl }}/images/6_spirals_mesh_render_03_018.png`

![test image]({{ site.baseurl }}/images/6_spirals_mesh_render_03_018.png)

This image links to a larger version of itself

[![node a]({{ site.baseurl }}/images/node_diagram_div_c_660.png)]({{ site.baseurl }}/images/node_diagram_div_c.png)

This images changes on mouse over - and has a caption

<a ><img src="{{ site.baseurl }}/images/overlay_example.png" onmouseover="this.src='{{ site.baseurl }}/images/overlay_example_texture.png'" onmouseout="this.src='{{ site.baseurl }}/images/overlay_example.png'" /></a>
*overlay with gray scale texture and color swatches*{.caption}


# Math

This block has no backslashes on the dollar signs

$$
K(a,b) = \int \mathcal{D}x(t) \exp(2\pi i S[x]/\hbar)
$$

this in line has double dollar signs $$d=\sqrt{\frac{4A}{\pi}}$$ no backslashes 

This block has backslashes everywhere

\$\$\\phi = \\frac{\\sqrt{5}+1}{2}\$\$

# Code 

A python code block using ` ```python `

```python
import pyb
             
# from adafruit arduino
init_cmds = [0xAE, 0xD5, 0x80, 0xA8, 0x3F, 0xD3, 0x0, 0x40, 0x8D, 0x14, 0x20, 0x00, 0xA1, 0xC8,
             0xDA, 0x12, 0x81, 0xCF, 0xd9, 0xF1, 0xDB, 0x40, 0xA4, 0xA6, 0xAF]
              
display_cmds = [0x21, 0, 127, 0x22, 0, 7]
              
## set up SPI bus              
rate = 8000000

spi = pyb.SPI(2, pyb.SPI.MASTER, baudrate=rate, polarity=0, phase=0)
dc  = pyb.Pin('Y4',  pyb.Pin.OUT_PP, pyb.Pin.PULL_DOWN)
res = pyb.Pin('Y3', pyb.Pin.OUT_PP, pyb.Pin.PULL_DOWN)
```

or using `{.python}`


```{.python}
for n in (0, 1, 2, 3, 4):
    print(n)
```


~~~{.python}
for n in (0, 1, 2, 3, 4):
    print(n)
~~~

Try some c code

```c
shader sphere(
    vector Vector = P,    
    float Radius = 0.5,
    vector Offset = 0,
    color ColorA = 1,
    color ColorB = 0,

    output color Color = 1,
    output float Fac = 0,
){
  float d = distance(Vector + Offset, vector(0, 0, 0));
  float inDisk = step(Radius, d);
  Color  = mix(ColorA, ColorB, inDisk);
  Fac = d;
}
```

# Tables

straight pipes

| a   | ui | op | we | 1 |
|-----|----|----|----|---|
| asd | b  | gh | kj | 2 |
| fgh | gh | c  | k  | 3 |
| zxc | jk | l  | d  | 4 |



# Internal links

[internal link]({{ site.baseurl }}{% link _posts/2014-12-27-simple_sverchok_02.md %})

