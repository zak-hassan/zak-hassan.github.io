---
title: "Profiling Python Programs"
date: 2020-12-10T11:59:44-07:00
draft: false
---


When profiling programs in python you will need to use a module called cProfile which will output a file which you can later use with Snakeviz. 

Snakeviz is a tool to see how well your code is performing. 

Another option to display the output is by using gprof2dot to convert the cprofile output into a graph.

```bash
python -m cProfile -o ~/prof.out app.py > cprof.txt
snakeviz ~/prof.out
Make A Graph Of Your Program:
gprof2dot -f pstats ~/prof.out -o graph.dot
dot -Tpng graph.dot -o graph.png
```