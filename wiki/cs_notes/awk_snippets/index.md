---
layout: page
title: awk snippets
math_support: mathjax
---


select the lines in the second file based on the first file

~~~ bash
awk 'NR==FNR { n[$1] = $2; next } ($1 in n) {print $0}' foo bar
~~~


