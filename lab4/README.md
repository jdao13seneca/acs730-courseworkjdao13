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

## CI Timings

| Measurement     | Baseline | Cache Miss | Cache Hit | Worth It? |
| pip install (Python 3.11) | 2 s |  3 s | 1 s  | Small improvement |
| Docker build step |         7 s | 19 s | 13 s | Not for this small image |
| Whole workflow |           41 s | 78 s | 71 s | Slower overall |

Pip cache: It reduced the Python 3.11 installation step from 
2 seconds to 1 second, but the overall improvement was minimal 
because the project has few dependencies.

Docker cache: Although the build step improved from 19 seconds to 13 seconds 
after reusing cached layers, it was still slower than the 7-second baseline, 
suggesting that cache and Buildx overhead were not worthwhile for 
this small application.
