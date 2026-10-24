## Build and Run

Build the optimized Docker image from the repository root:

```bash
docker build -t acs730-lab4:slim lab4/
```

Run the container:

```bash
docker run --rm -d --name lab4-app -p 8000:8000 acs730-lab4:slim
```

Check the health endpoint:

```bash
curl http://localhost:8000/healthz
```

Expected response: `{"status":"ok","version":"dev"}`

Stop the container:

```bash
docker stop lab4-app
```

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

## GHCR Image Tagging

I used SHA-based image tags instead of latest because each tag 
identifies a specific Git commit, making it easier to track which 
version of the application was built and deployed.

## Checkpoint 10 Choice Explanation

Composite Action: I used a composite action to combine Python setup, 
pip caching, and dependency installation into reusable steps that 
run on the same runner as the tests.

Reusable Workflow: I used a reusable workflow for Docker image building 
because it runs as a separate job with its own runner and permissions, 
including packages: write, which the test job does not require.


## GitHub Actions vs. AWS-Native CI/CD

GitHub Actions uses GitHub-hosted runners to run jobs, while AWS uses 
CodePipeline to manage the pipeline and CodeBuild to run the jobs. AWS 
also uses ECR to store Docker images instead of GHCR. For credentials, 
GitHub Actions uses "GITHUB_TOKEN" to publish images to GHCR, while AWS 
services rely on IAM roles and permissions. I would recommend GitHub 
Actions for this lab because we were able to build and test our 
pipeline without running into the IAM restrictions in AWS Academy, 
which would make setting up an AWS-native pipeline more difficult.


## Experiments

## Experiment 1 -  Invert the Layer Order

Prediction: I predicted that moving COPY app.py ./ before installing 
dependencies would make rebuilds slower because changing the 
application code would cause Docker to reinstall the dependencies.

Result: After changing app.py, the rebuild took 5.058 seconds, with 4.1 
seconds spent reinstalling dependencies. When I rebuilt without making 
another change, Docker reused the cached layers and completed the build 
in approximately 0.2 seconds. This showed me why placing the dependency 
installation before application code is more efficient for future 
rebuilds.

## Experiment 2 -  Removing .dockerignore

Prediction: I predicted that removing .dockerignore would increase the 
build context, but the final optimized image would stay around the same 
size because the Dockerfile only copies specific files.

Result: After removing .dockerignore, the build context was 473 bytes, 
and the final image remained 130 MB. When I checked /app, I only saw 
app.py and requirements.txt. This showed me that the optimized 
Dockerfile prevents unnecessary files from being included in the final 
image, unlike a Dockerfile using COPY . ., which could include 
additional files without .dockerignore.
