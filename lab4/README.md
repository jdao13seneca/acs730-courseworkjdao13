# Lab 4

Instructions for this section will be provided in class and on Blackboard when we reach it.

Put your work for Lab 4 in this folder.


## Docker Image Comparison

| Image | Base Image               | Size     | Rebuild Time |
| Naive | python:latest            | 1.15 GB  | 5.892 s |
| Slim  | python:3.12-slim-bookworm | 130 MB   | 0.620 s |


1. The difference is about 1.02GB, and this is because it uses the
slim-bookworm image instead of latest, which is a heavier image that comes with
more packages and tools.

2.The optimized image reused the cached dependency layer.

3. The .dockerignore file had excluded unnecessary files that were identified
in Part 4 Step 1, where we see it will ignore git data, markdown files, images.

