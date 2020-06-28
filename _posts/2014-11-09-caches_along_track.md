---
author: elfnor
date: '2014-11-09 22:00'
layout: post
tags:  geocaching
title: 'Off-line caching along a route'
---

I recently did a European cycle trip and wanted the opportunity to collect a few geocaches along the way. This post and the next [one on translation]({{ site.baseurl }}{% link _posts/2014-11-16-translate_caches.md %}), describe the tools I used to wrangle the data from [geocaching.com](http://www.geocaching.com) in to a GPX file and attempt to translate the cache descriptions and logs into English.

First the tools I use for geocaaching:

I use an android phone (rooted with cyanogenmod, but these tools all work on an unrooted phone) for geocaching. When I wanted something more suitable for geocaching than my old Garmin Etrex I looked at Garmin and other dedicated GPS, but decided a phone was more flexible and one third the price. My Motorola Defy+ is reasonably robust, and supposedly waterproof (not tested (yet!) although I use it happily in the rain). The GPS appears more accurate and works better than the Etrex when signals are weak (inside buildings) but can take along time to get a fix without access to the AGPS data via wifi.

I use off-line maps from [www.openandromaps.org](http://www.openandromaps.org). These are based on data from the [openstreetmap](http://www.openstreetmap.org) project. I use [Locus Pro](http://www.locusmap.eu/) for viewing the maps on the phone, but [c:geo](http://cgeo.org/) and [Orux maps](http://www.oruxmaps.com/index_en.html) are also very good.

Locus Pro can import caches stored in GPX format files.

Below I describe how I created a GPX file of the caches I expected to cycle past on my trip.

All this is described for running on a Linux machine with [geotoad](http://code.google.com/p/geotoad/) and [GPSBabel](http://www.gpsbabel.org/) installed, and assumes you\'re reasonably adept at working out how to use various bits of software.

The first step is to get a KML file of the the cycle trip route.

## Using Google maps to get a KML file of the route

The ways to save directions from Google maps and export them to a KML seems to vary and change depending on exactly which interface to Google maps you are using. The following worked on 2014-11-09.

-   go to <https://mapsengine.google.com/map/>
-   create a new map
-   select the icon for add directions
-   enter the start and end points for your cycle trip, and change the mode to cycle.
-   click on the folder icon near the top left, and select export to KML
-   select directions and download the file.
-   this file is a KMZ file which is just a zipped up KML file, extract the KML file using an appropriate archive tool

## Convert the KML file to a GPX file

-   Either use an on-line tool such as [kml2gpx](http://kml2gpx.com) or [gpsvisualizer](http://www.gpsvisualizer.com/)
-   Or use GPSBabel <http://www.gpsbabel.org/>.

## Other methods

An alternative using the ordinary Google maps interface and gpsbabel is described [here](http://www.gpsbabel.org/htmldoc-1.5.1/fmt_google.html).

[openrouteservice.org](http://www.openrouteservice.org) allows you to save a route directly as a GPX file, but only works for places in Europe.

## Get the caches along the route in the GPX file

-   Install geotoad and follow [these instructions](https://code.google.com/p/geotoad/wiki/OtherSearches#Searches_along_a_given_track/route)
-   Copy the geotoad instruction output by the script, add your own login name and password for geocaching.com, save this new command as a bash file, make executable and run.
-   The output of this last step should be a GPX file that can be importing into your caching software. It should contain all the caches within the specified distance of your cycle route.

See this [next blog post]({{ site.baseurl }}{% link _posts/2014-11-16-translate_caches.md %}) if you want to translate the cache descriptions and logs into English (or another language).

Note: Use geotoad at your own discretion it may not be strictly compatible with the terms of service of geocaching.com.

------------------------------------------------------------------------
