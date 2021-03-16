---
author: elfnor
date: '2014-07-26 22:00'
image: 'index_screen.png'
layout: post
tags:  webalbum
title: Responsive Theme for gthumb Web Albums
permalink: responsive-theme-for-gthumb-web-albums.html
---

I want to include a lot of the art works stuff I have on my [Deviant Art](http://elfnor.deviantart.com/) on this site. I\'m looking at the basic format of a static web album with a title, image and description for each art work. I looked at a number of packages including [sigal](https://github.com/saimn/sigal) and [llgal](http://home.gna.org/llgal/) but settled on [gthumb](https://wiki.gnome.org/Apps/gthumb).

Gthumb is mainly a image viewer and browser for the GNOME desktop, but it also includes a static web album generator. It has themes based on a set of templates and CSS files.

Of course I couldn\'t help fiddling with the templates and the Responsive Dark theme is the result. On a wide screen the description is to the right of the image, on a narrow screen the description is below the image.

The theme requires the description and title for each image to be entered into their respective exif tags. This is easy to do using gthumb itself. You can use some basic html such as `<br>` tags in the description field to give basic formatting.

To install the theme, download this [repository](https://github.com/elfnor/gthumb_responsive_theme) (see download zip button on the right of the github page). Unzip and copy the `Responsive_Dark` folder to the gthumb theme location. On my Linux Mint installation this is `/usr/share/gthumb/albumthemes/`

Start gthumb, choose the files for the web album, select `File>Export To>Web Album`. Select the `Responsive Dark` theme from the Theme box. Make sure `Adapt to the window width` is set on the `Index Page` tab.

Other settings I also used:

`Index Page` tab:

-   `Thumbnail Caption` set to `Title`
-   `All images on a single page` set

`Image Page` tab:

-   `Show the description, if available` set
-   `Show the following attributes:` set to `Title`

## Screenshots

### Index Page

### Image Page

![image screen shots]({{ site.baseurl }}/images/image_screen.png)

## Demo Album

There\'s a demo album [here](http://elfnor.github.io/artworksgallery/index.html).

------------------------------------------------------------------------
