# simple-flask-app-container-image-builder

A simple Flask application using Python 3 built via a `Dockerfile`. This repository serves as a practical guide to learning containerization basics.

> **Note:** Ignore the `ENV` setup inside the `Dockerfile` as setting persistent application configurations this way is a bad practice.

---

## 🚀 Quick Start Guide

### 1. Installation
Clone the repository and ensure Docker is installed and running:
```bash
git clone https://github.com/ksadithyan/simple-flask-app-container-image-builder.git
cd simple-flask-app-container-image-builder
```

### 2. Build the Image
Build the container image locally:
```bash
docker build -t adithyan/my-app .
```
*(The `-t` flag tags the image, and `.` the current directory)*.

### 3. Run the Container
Run the container with port mapping:
```bash
docker run -p 5000:5000 adithyan/my-app:latest
```

### 4. Verification
Access the endpoints in your browser:
*   `http://localhost:5000` → Welcome messages.
*   `http://localhost:5000/how-are-you` → Status message ("I'm fine. How are you?").

---

## ⚠️ Major Issues with Traditional Docker Builder
Traditional builds face efficiency and security challenges such as redownloading packages, secret leaks via `ENV`, `COPY`/`RM`, or `--build-arg`, architecture lock-in, and sequential execution of independent stages.

---

# 🔧 BuildKit

A modern approach that deals with the above issues.

```bash
docker buildx build -t adithyan/web-app .
```

Use these as fixes for the major issues above.

### 1. Slow rebuilds → cache mount
Caches downloaded files between builds and speeds up building.

```dockerfile
RUN --mount=type=cache,target=/root/.cache/pip <the command you need to run>
```
Change `target` to the cache folder of your tool (for example `pip`).

### 2. Secret leaks → secret mount
The secret is available only during that one `RUN` and leaves no trace in the image.

```dockerfile
RUN --mount=type=secret,id=mykey <the command you need to run>
```

When you build, pass the secret from outside. Here the key is in a `key.txt` file in the same folder:

```bash
docker buildx build --secret id=mykey,src=./key.txt -t myapp .
```

### 3. Architecture lock-in → multi-platform build
```bash
docker buildx build --platform linux/amd64,linux/arm64 -t ksadithyan/webapp .
```
(You can give any proper name.)

Finish the command with `--push` to push it to the registry under a single name as a multi-arch manifest:

```bash
docker buildx build --platform linux/amd64,linux/arm64 -t ksadithyan/webapp --push .
```

### 4. Sequential stages → parallel by default
BuildKit executes independent build stages side by side with zero additional effort.