---
title: "Profiling Python Programs"
date: 2021-07-10T14:42:58-07:00
draft: false
---

To run the python profiling code run the following:

. venv/bin/activate

# Will contain your env vars for LAD
source .env

python -m cProfile -o ~/prof.out app.py > cprof.txt

snakeviz ~/prof.out

Make A Graph Of Your Program:
gprof2dot -f pstats ~/prof.out -o graph.dot

dot -Tpng graph.dot -o graph.png