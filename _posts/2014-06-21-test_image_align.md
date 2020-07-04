---
author: elfnor
date: '2014-06-21 22:00'
image: 'El_Avatar2.jpeg'
layout: post
summary: 'Markdown synatx and main.css styling to float images to the left and right.'
tags:  pelican markdown
title: Image alignment with markdown
---

The standard syntax for inserting an image into a markdown document

```text
![avatar]({filename}/images/El_Avatar2.jpeg)
```

produces this html:

```html
<p><img alt="avatar" src="./images/El_Avatar2.jpeg" /></p>
```

and aligns an image to the left margin.

The text then continues underneath the image.

To float an image left or right so that the text flows around the image you can include a style in the attribute list after the image filename. Pelican automatically has the Attribute Lists extension to markdown enabled.

```text
![avatar]({filename}/images/El_Avatar2_face_left.jpeg){: style="float:right; margin: 20px 20px"}
```

produces this html:

```html
<p><img alt="avatar" src="./images/El_Avatar2_face_left.jpeg" style="float:right; margin: 20px 20px" /></p>
```

------------------------------------------------------------------------

![avatar]({{ site.baseurl }}/images/El_Avatar2_face_left.jpeg){: style=\"float:right; margin: 20px 20px\"}

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum

------------------------------------------------------------------------

![avatar]({{ site.baseurl }}/images/El_Avatar2.jpeg){: style=\"float:left; margin: 20px 20px\"}

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

------------------------------------------------------------------------

Better practive would be to define a pair of styles in the `main.css` file:

```css
/* Images */
.floatright {
    float: right;
    margin: 20px 20px;
    border: 2px solid #ccc;
}

.floatleft {
    float: left;
    margin: 20px 20px;
    border: 2px solid #ccc;
}
```

Then reference this in the markdown as follows:

```text
![avatar]({filename}/images/El_Avatar2_face_left.jpeg){: .floatright}
```

------------------------------------------------------------------------

![avatar]({{ site.baseurl }}/images/El_Avatar2_face_left.jpeg){: .floatright}

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum

------------------------------------------------------------------------

There is a pelican plugin called [Better Figures and Images](http://duncanlock.net/blog/2013/05/29/better-figures-images-plugin-for-pelican/) but it seems only designed for writing in reStructured Text not markdown.
