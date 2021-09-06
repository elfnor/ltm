---
author: elfnor
date: '2021-06-09 22:00'
image: 2021-06-09-header-celtic-knot-font.png
layout: post
tags: krita
title: "Celtic Knot Font"
permalink: 2021-06-08-celtic-knot-font.html
---

Stumbling around on another project I came across a link on the Wikipedia entry on [Celtic Knots](https://en.wikipedia.org/wiki/Celtic_knot) to  [MrBenGriffin's Celtic Knotwork Generator](https://github.com/MrBenGriffin/Knot). I was intrigued. 

I have little idea how ligatures and fonts work, but I could get this all going fairly simply. I'll detail my exploration here as I found working out what was going on by trial and error rather fun.

I did this using Linux Mint and LibreOffice but other OS and software should work.

Download the zip file of the font from [fontlibrary](https://fontlibrary.org/en/font/knots) or get it with the github repo above. Install it on your system. Right click on one of the font `*.otf` files `Open with Fonts` and click `Install`. (I'm using `KnotsZoo-Rustic.otf` for these examples)

Open LibreOffice Writer and select the `KNOTS Zoo` font and set the font size to say 40.

Start typing a random string of "i" an "o" characters. See if you can figure what's going on.

![gif](../images/2021-06-09/celtic_knot_02.gif)

After a while you should see that a string of four characters produces a glyph.

![glyphs with strings](../images/2021-06-09/glyph_strings.png)

After some more experimenting you'll find that each character in the string of four refers to one side of the glyph, starting at the top and going clockwise.

![glyphs with characters](../images/2021-06-09/glyph_strings_02.png)

An "i" has a connection to the next glyph, an "o" has no connection on that side.

Now see if you can produce this pattern (hint: "oooo" is a blank glyph).

![quiz 01](../images/2021-06-09/quiz_01.png)

What about this one?

![quiz 02](../images/2021-06-09/quiz_02.png)

Now start adding "x" characters to the strings.

![glyph_oix](../images/2021-06-09/border_oxi.png)

The full list of characters available is "hibox". "h" gives heads, and "b" beaks or spirals.

![glyph_hibox](../images/2021-06-09/border_hibox.png)

For designing patterns LibreOffice Calc is good with four characters or one glyph per cell. its useful in the design stage as you can see the character string in the input line at the same time as you can see the glyph in the cell. You can also easily edit glyph by glyph.

![calc snapshot](../images/2021-06-09/calc_screenshot.png)

The project's github repository includes a python script to produce patterns with particular dimensions and symmetries. It outputs a sting of characters to the terminal that can be pasted back into your text editor or used on a webpage.

You can also use this font to play with other patterns such as the plane symmetry groups. [Wallpaper group - Wikipedia](https://en.wikipedia.org/wiki/Wallpaper_group)) Or see my old [blog entry](https://elfnor.com/Symmetry%20Group%20Links%20and%20Notation.html)

This one has p2 symmetry. 

![p2-symmetry](../images/2021-06-09/p2-symmetry.png)

Now I could make a font for playing Escher's Potato Game... [Combinatorial enumeration of 2×2 ribbon patterns - ScienceDirect](https://www.sciencedirect.com/science/article/pii/S0195669806000746))

Play that game over here [Escher Tiles - Interaction](http://www.eschertiles.com/interaction.html)
