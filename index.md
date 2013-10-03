---
layout: page
title: cranvas
tagline: Interactive Statistical Graphics with R and Qt
---
{% include JB/setup %}

The R package `cranvas` aims to be the next generation of GGobi, a software
package for interactive and dynamic statistical graphics. It includes most
of features in GGobi such as brushing, zooming, panning, identifying and
linking, as well as common types of statistical graphics, e.g., bar plot,
scatter plot, boxplot, histogram, density plot, spine plot, parallel
coordinates plot, mosaic plot, maps, missing value plot, time series plot,
tour, scatter plot matrix, hexagons and tiles (color images), etc. Based on
the support of several other packages, cranvas aims for speed (from Qt) and
flexibility (from R), with the style and design borrowed from `ggplot2`.

<iframe src="http://www.screenr.com/embed/9Le8" width="650" height="396" frameborder="0"></iframe>

## Brushing

<iframe src="//player.vimeo.com/video/30173477" width="500" height="516" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

## Identifying

<iframe src="//player.vimeo.com/video/16634690" width="500" height="346" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

## Latest Posts

<ul class="posts">
  {% for post in site.posts limit:5 %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
