Zolan - Modern & Minimal Theme for Jekyll
======
Zolan is a minimal blog theme for Jekyll.

* * *

Table of Contents
-----------------
*   [Features](#features)
*   [Demo](#demo)
*   [Deployment](#deployment)
*   [Posts](#posts)
*   [Disqus Comments](#DisqusComments)
*   [Instagram](#instagram)
*   [Google Analytics](#GoogleAnalytics)
*   [Update favicon](#UpdateFavicon)
*   [Credits](#Credits)
*   [Support](#Support)

* * *

### Features

* 100% responsive and clean theme

* Optimized for mobile devices

* Minimal design

* Valid HTML5 code

* Post sharing

* Subscription form

* Supports Disqus Comments

* Instagram Feed

* Ionicons Icons

* Google Fonts


* * *

### Demo

Check the theme in action [Demo](https://zolan-jekyll.netlify.app/)

![Main page preview](https://github.com/artemsheludko/zolan/blob/master/images/zolan-main-page.png?raw=true)

The post page would look like this:

![Post page preview](https://github.com/artemsheludko/zolan/blob/master/images/zolan-post.png?raw=true)

* * *

### Deployment

To run the theme locally, navigate to the theme directory and run `bundle install` to install the dependencies, then run `jekyll serve` or `bundle exec jekyll serve` to start the Jekyll server.

I would recommend checking the [Deployment Methods](https://jekyllrb.com/docs/deployment-methods/) page on Jekyll website.

* * *

### Posts

To create a new post, you can create a new markdown file inside the \_posts directory by following the [recommended file structure](https://jekyllrb.com/docs/posts/#creating-post-files).

      ---
      layout: post
      title: Time to give gifts to everyone
      date: 2018-08-23 16:04:00 +0300
      image: 03.jpg
      tags: Life
      ---


You can set the tags and the post image.

Add post images to **/images/** directory.

For tags, try to not add space between two words, for example, `Ruby on Rails`, could be something like (`ruby-on-rails`, `Ruby_on_Rails`, or `Ruby-on-Rails`).

* * *

### Disqus Comments

Zolan Theme comes with Disqus comments enabled.

Open `_data/settings.yml` file, and change the `mr-brown` value on line 26 with your [Disqus account shortname](https://help.disqus.com/customer/portal/articles/466208).

      Comment Section (Disqus)
      disqus-identifier: mr-brown # Add your shortname for Disqus Comment. For example mr-brown


That’s all you need to setup Disqus from the theme side. If you get any issue regarding that comments are unable to load. First, make sure you have [registered your website with Disqus (Step 1)](https://help.disqus.com/customer/portal/articles/466182-publisher-quick-start-guide).

And also check [Disqus troubleshooting guide](https://help.disqus.com/customer/portal/articles/472007-i-m-receiving-the-message-%22we-were-unable-to-load-disqus-%22) if you still have issues.

* * *

### Instagram

The Instagram feed is working using [Instafeed.js](http://instafeedjs.com/) to show the photos.

First, you will need to get your account `userId` and `accessToken` from the following URLs:

*   userId: [http://codeofaninja.com/tools/find-instagram-user-id/](http://codeofaninja.com/tools/find-instagram-user-id/)
*   accessToken: [instagram.pixelunion.net](http://instagram.pixelunion.net/)

Second, open the `js/common.js` file and replace the `userId` and `accessToken` values.

    var instagramFeed = new Instafeed({
          get: 'user',
          limit: 6,
          resolution: 'standard_resolution',
          userId: '8987997106',
          accessToken: '8987997106.924f677.8555ecbd52584f41b9b22ec1a16dafb9',
          template: ''
    });


Third, open the `_data/settings.yml` file and replace the `instafeed: false` on `instafeed: true` value.

    # Instagram Feed
    instafeed: false # To enable the instafeed, use the value true. To turn off use the value false.


* * *

### Google Analytics

To integrate Google Analytics, open `_data/settings.yml`, and add your Google Analytics identifier.

    # Google Analytics
    google-analytics: # Add your identifier. For example UA-99631805-1


* * *

### Update favicon

You can find the current favicon (favicon.ico) inside the theme root directory, just replace it with your new favicon.

* * *

### Credits

I have used the following scripts, fonts or other files as listed.

*   [Google Fonts](https://fonts.google.com/specimen/Nunito) (Roboto, Sans Serif).
*   [Ionicons Icons](https://ionicons.com/)
*   [FitVids.js](http://fitvidsjs.com/)
*   [Medium’s Image Zoom](https://github.com/fat/zoom.js)
*   [Instafeed.js](http://instafeedjs.com/)
*   [jQuery.com](https://jquery.com/)
*   Preview Images form [unsplash.com](https://unsplash.com/), [pexels.com](https://www.pexels.com/)

* * *
### License

Mit License

* * *






### elfnor's notes on changes
{2020-06-05 14:15}
added  background image in
`_includes/head.html`

removed logo and added title in
`_data/settings.yml`

removed hero section in

`index.html`

added
```
$red: #711;
$lightred: #c14c4c;
```

And changed heading colours to $red, also heading size

in
`_sass/0-settings/_variables.scss`

and changed `$brand-color: #c14c4c;` which changes the tag colour

Changed `.post-image background-size` to `auto` in `_sass/4-layouts/_post.scss`


banner colour and font

Add rock salt to fonts in `_includes/head.html` and add `font-family: 'Rock Salt', cursive;` to `  .logo__link` in `_sass/3-modules/_header.scss`

`.header` and `.logo` changes to position header text

get rid of style guide , remove file `_pages/styleguides.md`

copy some of the parameters for the cards on the index page to `.post`
make all headers $red

make white box on tag page transparent and smaller, change font to uppercase etc.

less white space around images on posts - `.post margin`, `.post-title {
  margin-bottom: 0px;` and some padding somewhere

remove subscribe etc on index page

fix footer stuff - the image on the last page is messed?
image - is messed on original too, add a blank image to fill the spot?
added transparent placeholders for navigation images in `_layouts/post.html`
```
{% else %}
<div class="prev" style="opacity: 0"> </div>
{% endif %}
```

remove share this article

add access to a list of all tags - page? menu item? look at other sites
tags done
styling for image captions


code pigments

liquid tags works but to use triple backticks (fenced code blocks)
add to `_config.yml`
```
highlighter: rouge
markdown: kramdown
kramdown:
  input: GFM
  auto_ids: true
  syntax_highlighter: rouge
```  

needs small `c` not `C` for highlighting c code or OSL

only the food tag works? others are 404? , all lowercase tags work, others don't

Tables
the table in the literary voyage post is broken
adding a line between the heading and the table fixes this, now do CSS. All the cute icon links work. Can't find any table css, create `_sass/3-modules/_tables.scss` and copy old blog syntax, works , but column spacing is odd on narrower page format - leave for now - one layout would be to put the title and comment in a single cell but on separate lines, but not sure markdown can do this.

Maths done, using $$ for both inline and display math.

TODO















maths - hyperbolic tiling in 3D post - hasn't worked, nor has float right style on image.

Using this post http://benlansdell.github.io/computing/mathjax/ I can inline maths to work with single $ but not display math with $$



This version needs `$$` for inline https://quuxplusone.github.io/blog/2018/08/05/mathjax-in-jekyll/

This is probably OK as it avoids need to escape ordinary $ signs.





https://github.com/jeffreytse/jekyll-spaceship
addon requires using jekyll locally and uploading html to gh-pages, same as I do now.

https://karas.io/blog/math-support-with-katex-on-github-pages/
https://github.com/stevenkaras/stevenkaras.github.com
this uses some katex and javascript

https://github.com/stevenkaras/stevenkaras.github.com/blob/master/_includes/js/katex.js



yml front matter needs lowercase for id
all posts need the `---` added and  a `layout` specified

image links needs to be of form
`![noodle + render]({{ site.baseurl }}/images/blender_osl_shaders-b2b2a.png)`

instead of
`![noodle + render](images/blender_osl_shaders-d3538.png)`

This can easily be done with search and replace

navigation image cropped? sometimes, sometimes not




transfer sample posts especially ones with lots of images, tables, code, math
one without an image etc. or some where I wrapped the text around the image






Tag Clouds in Jekyll
https://hyunyoung2.github.io/2016/12/17/Tag_Cloud/
https://www.stefan.lu/blog/tag-cloud-in-jekyll-no-plugins/
https://vvv.tobiassjosten.net/jekyll/jekyll-tag-cloud/
https://jovandeginste.github.io/2016/05/04/add-a-tag-cloud-to-my-jekyll-site.html
https://www.gungorbudak.com/blog/2017/12/08/tags-cloud-sorted-by-post-count-for-jekyll-blogs-without-plugins/

I like the idea of a reading list page, listing a small number of links to articles or books.


TODO
make the page css/layout closer to the post version
remove google analytics, discuss etc, from theme at code level
test deployment on github
redo about page, galleries page
redo 404 page
remove unused css etc. -  I think the scss stuff does this
put in site analytics same as current blog
automate transfer of remaining posts - see below.

Write a post about editing a theme and transferring from Pelican to Jekyll.



### Changes to implement across all posts.
see `/home/elfnor/beasthoard/Projects/markdown_pandoc` for doing the image links and inline maths conversion with pandoc.
and `/home/elfnor/beasthoard/Projects/markdown_pandoc/temp-md/pelican-to-jekyll.ipynb` for some of the python conversions. Still need to write file rename

rename files with date from metadata

https://www.jamesleighton.com/2018/10/06/migrating-pelican-to-jekyll/
but folder level find and replace will probably work.

all posts need the `---` added and  a `layout` specified  
yml front matter needs lowercase for ids

image links needs to be of form  
`![noodle + render]({{ site.baseurl }}/images/blender_osl_shaders-b2b2a.png)`

replace single $ signs with $$ in inline maths  

move post image to front matter - this one is more than simple search and replace so will write python script.


Goatcounter looks like a light open source analytics solution
This is how to connect the old posts to the new posts.
https://www.jessesquires.com/blog/2020/04/15/updating-permalinks-and-adding-redirects-for-jekyll-sites/
Should include this in script but htaccess is not available on github pages

this plugin works on github pages using a single redirect file
https://github.com/nquinlan/jekyll-pageless-redirects

this plugin has redirects in each post front matter, but generates lots of extra pages
https://github.com/tsmango/jekyll_alias_generator

make sure I use https https://github.blog/2018-05-01-github-pages-custom-domains-https/
