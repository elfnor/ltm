---
author: elfnor
date: '2014-06-19 22:00'
layout: post
tags: pelican markdown
title: 'Pelican and markdown styling cheat-sheet'
---

A [Pelican](http://docs.getpelican.com) blog consists of:

-   **markdown** text files containing **content**
-   \_\_ jinga2\_\_ html template files defining the pages **layout**
-   **CSS** style file that determine how the page elements **look**

These elements are processed by the Pelican python software to produce the static html pages that make up this blog.

Lots of blogs (I used [this one](http://www.circuidipity.com/github-pages.html) mainly) have very good information on installing Pelican. Many [for example](http://www.krisyu.org/blog/posts/2013/06/markdown-and-latex-reference) also have the markdown codes used in text files to define what parts of the text are code blocks, what parts are quotes. But I couldn\'t find a post that described where and how to edit the CSS style files that determine how the the different elements look on the finished blog.

I\'ll try and write that post.

A warning: I\'m fairly new and no expert on html or CSS and most of this information comes from poking pelican themes and seeing what happens. Most of this should apply to any Pelican theme, but mostly I\'ll be fiddling about with the pelican-simplegrey theme.

## In-line Code

This is text such as `this` that is normally used for a short piece of code in a a paragraph. It is enclosed in back quote marks, the key in the extreme top left of a US keyboard.

It might also be used when a variable name or file name `~\file.txt` is referred to in a paragraph.

``` {.text}
This is text such as `this` that is normally...
```

The static html produced by Pelican is:

``` {.html}
<p>This is text such as <code>this</code> that is normally used for a short piece of code in a a paragraph. </p>
```

This format of the in-line code element is given by a piece of CSS such as this, included in the theme `main.css` or `style.css` file

``` {.css}
code {
  font-family: 'Source Code Pro', monospace;
  font-size: 0.9em;
  font-style: normal;
  letter-spacing: 0.015em;
}
```

## Code Blocks

This is a block set of from the main text, normally containing a longer piece of code.
The blocks can be defined in a number of different ways, indenting, four `~~~~`, but I\'ll show examples with triple back quote marks (\`\`\`).

\~\~\~\~{.text}

    # python code  
    mystr = 'Hello world'  
    print('%s'%(mystr))  

\~\~\~\~

produces this html:

``` {.html}
</p>
<div class="highlight"><pre><span class="c"># python code</span>
<span class="n">mystr</span> <span class="o">=</span> <span class="s">&#39;Hello world&#39;</span>
<span class="k">print</span><span class="p">(</span><span class="s">&#39;</span><span class="si">%s</span><span class="s">&#39;</span><span class="o">%</span><span class="p">(</span><span class="n">mystr</span><span class="p">))</span>
</pre></div>
```

and this output

``` {.python}
# python code
mystr = 'Hello world'
print('%s'%(mystr))
```

The style for the `pre` element is defined in the `main.css`.

``` {.css}
pre {
  font-family: 'Source Code Pro', monospace;
  background: none repeat scroll 0 0 #F0F0F0;
  border-radius: 2px;
  font-size: 0.9em;
  font-style: normal;
  letter-spacing: 0.015em;
  line-height: 130%;
  padding: 0.7em;
  white-space: pre-wrap;
  word-wrap: break-word;
}
```

and mostly defines fonts and backgrounds. All the span elements are produced by pygments code and coloured according to the css normally stored in `pygments.css`. The easiest way to change this is to use a predefined css from another style (pelican-bootstrap3 has lots).

To add line numbers to the displayed code block use `#!` at the start of the first line of code:

\~\~\~\~{.text}

``` {.python}
#! python code  
mystr = 'Hello world'  
print('%s'%(mystr))  
```

\~\~\~\~

    #! python code  
    mystr = 'Hello world'  
    print('%s'%(mystr))  

## Block Quotes

Blockquotes can be indicated with \>

> Here\'s a blockquote.

> This a much longer multi paragraph block quote that is intended to extend over multiple lines. This a much longer block quote that is intended to extend over multiple lines. This a much longer block quote that is intended to extend over multiple lines.

``` {.text}
> Here's a blockquote.

> This a much longer multi paragraph block quote that is intended to extend over multiple lines.  This a much longer block quote that is intended to extend over multiple lines.  This a much longer block quote that is intended to extend over multiple lines. 
```

produced by this html:

``` {.html}
<blockquote>
<p>Here's a blockquote.</p>
<p>This a much longer multi paragraph block quote that is intended to extend over multiple lines.  This a much longer block quote that is intended to extend over multiple lines.  This a much longer block quote that is intended to extend over multiple lines. </p>
</blockquote>
```

The style for the `blockquote` element is defined in the `main.css`.

``` {.css}
blockquote {
    font-style: italic;
    margin-top: 10px;
    margin-bottom: 10px;
    margin-left: 20px;
    padding-left: 15px;
    border-left: 3px solid #ccc;
}
```

## Attribute lists

If you want to have several different types of an element, each with its own style, you can use attribute lists.

``` {.text}
For example different coloured in-line `code in red`{: .red} and `green`{: .green}.
```

For example different coloured in-line `code in red`{: .red} and `green`{: .green}.

Html:

``` {.html}
<p>For example different coloured in-line <code class="red">code in red</code> and <code class="green">green</code>.</p>
```

Along with the following css in `main.css`.

``` {.css}
code.red {color:red;}
code.green {color: green;}
```

## Headings

Headings can be made in markdown by underlining with equal signs (===========) to give h1 tags, underlining with underscores (\-\-\-\-\-\-\-\--) to give h2 tags, or by enclosing the headings with hash (\#Heading\#) symbols. The number of hash symbols gives the heading depth.

# H1

# H1

## H2

## H2

### H3

markdown

``` {.text}
#H1#
H1
==
##H2##
H2
--
###H3###
```

HTML

``` {.html}
<h1>H1</h1>
<h1>H1</h1>
<h2>H2</h2>
<h2>H2</h2>
<h3>H3</h3>
```

CSS

``` {.css}
h1, h2, h3, h4, h5, h6 {
    color: #711;    
    font-family: "Source Sans Pro",sans-serif;
    font-weight: 400;
    text-shadow: 0.1em 0.1em 0.1em #EFEFEF;
    line-height: 125%;
}

h1 {
  font-size: 2em;
  margin: 0.67em 0;
  padding: 0.7em 0 0.3em;
}
```

## Learning more

I could carry on with examples for styling other elements such as lists and links, but the learning process is fairly straight forward.
On your blog, or someone else\'s, right click on an element (Firefox or Chrome) and choose inspect element. A window opens at the bottom of the screen with the html for the element highlighted on the left and the style css highlighted on the right. If the element has its own css, look that up in your theme\'s css files and alter it to suit. A web search will give you lots of examples for styling each element. If all of the css is inherited from `body` for your chosen element then look at the tags that wrap the element in the html file. Add a section to the css for that type of element tag and continue as above.

I\'ll follow this up with a post on the basics of layout using Jinja2 templates.
