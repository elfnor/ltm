---
author: elfnor
date: 2024-08-09 00:00
image: 2024-08-09-header-new-gallery.png
image.path: /images/2024-08-09-header-new-gallery.png
layout: post
tags:
  - blog
title: New Gallery using Thumbsup
permalink: 2024-08-09-new-gallery.html
---

I've added a link to my new static photo  [gallery](https://elfnor.github.io/elfnor-gallery/index.html) in the blog header.

I mainly wanted somewhere to share some of my photography away from any of the social media sites. I've also collected up some albums up of my old clay sculptures (dormant [deviant art](https://www.deviantart.com/elfnor) profile) and some digital art that's also on ([ArtStation](https://www.artstation.com/elfnor)).

The gallery deployment needs to be really low effort for me to (hopefully- good intentions) keep updating it. I looked at some nice jekyll themes ([photorama](https://github.com/sunbliss/photorama), [starving-artist](https://github.com/chrisanthropic/starving-artist-jekyll-theme), [photography](https://github.com/rampatra/photography)) but settled instead on [thumbsup](https://github.com/thumbsup/thumbsup) a standalone  static HTML generator.

The gallery is deployed using the workflow from [github-pages-gallery](https://github.com/gautamkrishnar/github-pages-gallery) to setup github actions to host it directly on Github pages.

The [theme](https://gitlab.com/langurmonkey/langurmonkey.gitlab.io/-/tree/master/gallery-theme) is adapted from one by [langurmonkey](https://tonisagrista.com/blog/2021/static-photo-gallery/), with a bit of styling to match this blog.

Adding new photos is as simple as:
- copy files into  folders/albums in the local git repository for https://github.com/elfnor/elfnor-gallery
- (optional) add a description via the  comment button in [gthumb](https://wiki.gnome.org/Apps/Gthumb) or similar.
- (optional) run `docker run -v "$(pwd):/work" ghcr.io/thumbsup/thumbsup /bin/sh -c "cd /work/ && thumbsup --config config.json"` to build locally 
- add-commit-push to Github 
- DONE!

There's a few things I'd like to improve:
- the description banner is a bit intrusive
- and I'd like longer descriptions for some of the artworks - could I reuse the right-hand panel designed for exif info?
- match the styling a little closer to the blog.
- do I want to strip (some/all) exif data? Or show it?

