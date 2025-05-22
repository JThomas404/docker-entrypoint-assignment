# Docker ENTRYPOINT Assignment 01 Documentation

This project was completed as part of the **[Docker Mastery Course](https://www.udemy.com/course/docker-mastery/)** by Bret Fisher.

The assignment demonstrates how to containerise simple CLI programs using Docker’s `ENTRYPOINT` instruction. It focuses on building lightweight, purpose-driven containers that execute a single command-line application with default behaviour that can be customised at runtime.

---

## 📁 Project Structure

```
entrypoint-assignment-01/
├── cmatrix/
│   ├── Dockerfile
│   └── README.md
├── apachebench/
│   ├── Dockerfile
│   └── README.md
└── README.md (project overview)
```

---

## Assignment Overview

You were tasked with creating two standalone Dockerfiles:

1. **CMatrix CLI Screensaver**
   A fun terminal-based matrix-style screensaver that runs with visual arguments and requires a terminal session (`-it`).

2. **ApacheBench (ab) Web Benchmarking Tool**
   A containerised version of Apache’s `ab` tool used to benchmark HTTP server performance, designed to run with optional flags and default to `--help` output if no arguments are passed.

---

## Part 1: CMatrix

* **Base Image**: `alpine:3.21.3`
* **Installed Tool**: `cmatrix`
* **ENTRYPOINT**: `cmatrix`
* **CMD**: `-abs -C red`
* **Usage Note**: Requires `-it` to render in terminal.

**Example Usage:**

```bash
docker build -t zermatrix ./cmatrix
docker run -it zermatrix -C green -b
```

---

## Part 2: ApacheBench

* **Base Image**: `ubuntu:24.04`
* **Installed Tool**: `apache2-utils` (for `ab`)
* **ENTRYPOINT**: `ab -n 10 -c 2`
* **CMD**: `--help`
* **Usage Note**: By default, it shows help unless a URL is passed at runtime.

**Example Usage:**

```bash
docker build -t apachebench ./apachebench
docker run -it apachebench https://connectingthedotscorp.com/
```

---

## Validation Steps

* Ensure all Dockerfiles build successfully.
* Run each container with and without overriding `CMD`.
* Visit the GitHub repository and verify correct file structure.

---

## Credits

This project is a hands-on assignment from the [Docker Mastery Course](https://www.udemy.com/course/docker-mastery/) by Bret Fisher, aimed at reinforcing Docker fundamentals through real-world examples.

---