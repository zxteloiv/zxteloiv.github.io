---
layout: page
title: mathematica plot
math_support: mathjax
---


## single column

~~~ mathematica
data = ReadList["~/Downloads/lossiter.txt"]; ListLinePlot[data]
~~~

## multiple data file containing single column

~~~ mathematica
Rasterize[
 ListLinePlot[{ReadList["~/Downloads/graph.log.1.2"], 
   ReadList["~/Downloads/graph.log.12"]}, 
  PlotLegends -> {"eta=.1", "eta=.12"}]]
~~~




