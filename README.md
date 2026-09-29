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
