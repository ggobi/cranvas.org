---
layout: post
title: "The Performance of Qt in R"
description: ""
categories: [Qt]
tags: [brushing, scatterplot]
---
{% include JB/setup %}

Qt can be incredibly fast when drawing a huge number of graphical elements.
Below is a video showing the speed of brushing a scatterplot of three
million points:

<iframe src="http://www.screenr.com/embed/f86H" width="500" height="500" frameborder="0"></iframe>

And the code for this example:

{% highlight r %}
n = 3000000; r = 40
df = data.frame(x=rnorm(n,300,60), y=rnorm(n,300,40))

library(SearchTrees)
tree = createTree(df)
idx = NULL; pos = NULL
library(qtpaint); library(qtbase)

# ready? draw!
s = qscene()
root = qlayer(s)
mouse_move = function(layer, event) {
  pos <<- as.numeric(event$pos())
  idx <<- rectLookup(tree, pos, pos + r)
  qupdate(layer.brush)
}
lims = qrect(apply(df, 2, range))
layer.main = qlayer(
  paintFun = function(layer, painter) {
    qdrawGlyph(painter, qglyphCircle(1), df$x, df$y, fill = "black",
               stroke = "black")
  },
  clip = TRUE, cache = TRUE, mouseMoveFun = mouse_move, mousePressFun = mouse_move,
  limits = lims
)
layer.brush = qlayer(
  paintFun = function(layer, painter) {
    qdrawRect(painter, pos[1] + r, pos[2] + r, pos[1], pos[2], stroke = "red",
              fill = NA)
    if (!length(idx)) return()
    qdrawGlyph(painter, qglyphCircle(5), df$x[idx], df$y[idx], fill = "yellow",
               stroke = "yellow")
  },
  limits = lims
)
root[0, 0] = layer.main
root[0, 0] = layer.brush
system.time({
  v = qplotView(s)
  v$resize(500, 500)
  print(v)
})

# Qanviz::PlotView instance
#   user  system elapsed
#  0.000   0.000   0.005
{% endhighlight %}

The hardware information:

    Processor: 8x Intel(R) Core(TM) i7-3630QM CPU @ 2.40GHz
    Memory: 7991MB
    Operating System: Ubuntu 13.10
    OpenGL Renderer: Mesa DRI Intel(R) Ivybridge Mobile
    X11 Vendor: The X.Org Foundation

