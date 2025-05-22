# 🐳 Docker ENTRYPOINT Assignment 01 Documentation

This project was completed as part of the **[Docker Mastery Course](https://www.udemy.com/course/docker-mastery/)** by Bret Fisher.

The assignment demonstrates how to containerise simple CLI programs using Docker’s `ENTRYPOINT` instruction. It focuses on building lightweight, purpose-driven containers that execute a single command-line application with default behaviour that can be customised at runtime.

---

## Project Structure

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

## Part 1: CMatrix – Build & Run

**Dockerfile Highlights:**

```Dockerfile
FROM alpine:3.21.3

RUN apk add --no-cache cmatrix

ENTRYPOINT [ "cmatrix" ]
CMD [ "-abs", "-C", "red" ]
```

---

### Build the Image

```bash
cd cmatrix
docker build -t zermatrix .
```

---

### Run the Container

```bash
docker run -it zermatrix
docker run -it zermatrix -C green -b
```
> Note: `-it` is essential for `cmatrix` to render in your terminal correctly.

---

## Part 2: ApacheBench – Build & Run

**Dockerfile Highlights:**

```Dockerfile
FROM ubuntu:24.04

RUN apt-get update \
    && apt-get install -y apache2-utils \
    && rm -rf /var/lib/apt/lists/*

ENTRYPOINT [ "ab", "-n", "10", "-c", "2" ]
CMD [ "--help" ]
```

---

### Build the Image

```bash
cd apachebench
docker build -t apachebench .
```

---

### Run the Benchmark

**Default help output:**

```bash
docker run -it apachebench
```

---

**Run benchmark on a live site:**

```bash
docker run -it apachebench https://connectingthedotscorp.com/
```

> `-n 10` = 10 requests
> `-c 2` = 2 concurrent requests

---

### ApacheBench Output

```text
This is ApacheBench, Version 2.3 <$Revision: 1903618 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking connectingthedotscorp.com (be patient).....done


Server Software:        AmazonS3
Server Hostname:        connectingthedotscorp.com
Server Port:            443
SSL/TLS Protocol:       TLSv1.3,TLS_AES_128_GCM_SHA256,2048,128
Server Temp Key:        X25519 253 bits
TLS Server Name:        connectingthedotscorp.com

Document Path:          /
Document Length:        0 bytes

Concurrency Level:      2
Time taken for tests:   1.425 seconds
Complete requests:      10
Failed requests:        0
Non-2xx responses:      10
Total transferred:      3903 bytes
HTML transferred:       0 bytes
Requests per second:    7.02 [#/sec] (mean)
Time per request:       284.929 [ms] (mean)
Time per request:       142.464 [ms] (mean, across all concurrent requests)
Transfer rate:          2.68 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:       41   75  36.1     80     159
Processing:    11  124 133.5     54     371
Waiting:        9  115 126.8     51     354
Total:         63  200 146.4    151     530

Percentage of the requests served within a certain time (ms)
  50%    151
  66%    228
  75%    246
  80%    353
  90%    530
  95%    530
  98%    530
  99%    530
 100%    530 (longest request)
```

---

### Detailed Breakdown & Interpretation

#### **Header**

* `ApacheBench, Version 2.3` – The version of the benchmarking tool.
* Shows licensing info from the Apache Software Foundation.

---

#### **Target Details**

* **Benchmarking connectingthedotscorp.com** – The target URL.
* **Server Software**: `AmazonS3` – Indicates the server is backed by AWS S3 (likely a static website).
* **Server Hostname**: `connectingthedotscorp.com`
* **Server Port**: `443` – HTTPS.
* **TLS Info**: Shows the cipher and protocol used for secure communication.

---

#### **Document Info**

* **Document Path**: `/` – Root path of the site.
* **Document Length**: `0 bytes` – No actual HTML body content returned (e.g., empty or just headers).

---

#### **Test Parameters**

* **Concurrency Level**: `2` – Two simultaneous requests at any time.
* **Time Taken for Tests**: `1.425s` – Total test duration.
* **Complete Requests**: `10` – Total requests made.
* **Failed Requests**: `0` – All succeeded in terms of connectivity.
* **Non-2xx Responses**: `10` – None of the responses returned HTTP 2xx status codes (likely 403, 301, etc.).

---

#### **Traffic Stats**

* **Total Transferred**: `3903 bytes` – Total bytes including headers.
* **HTML Transferred**: `0 bytes` – No actual HTML body received.
* **Requests per Second**: `7.02 [#/sec]` – Throughput.
* **Time per Request (mean)**:

  * `284.929 ms` – Average time for each request (sequential).
  * `142.464 ms` – Adjusted for concurrency (i.e., per concurrent thread).
* **Transfer Rate**: `2.68 KB/s` – Average download rate.

---

#### **Connection Times (Milliseconds)**

* **Connect** – Time to establish a TCP+TLS connection.
* **Processing** – Time taken to send request and wait for full response.
* **Waiting** – Time spent waiting after sending the request (similar to TTFB – Time to First Byte).
* **Total** – Entire round-trip duration.

---

| Metric     | Min | Mean | Median | Max |
| ---------- | --- | ---- | ------ | --- |
| Connect    | 41  | 75   | 80     | 159 |
| Processing | 11  | 124  | 54     | 371 |
| Waiting    | 9   | 115  | 51     | 354 |
| **Total**  | 63  | 200  | 151    | 530 |

> The variability in total time (`min=63ms`, `max=530ms`) reflects network jitter, DNS or TLS negotiation delays, or server-side latency.

---

#### **Percentiles**

Shows how many requests completed under certain durations:

* `50% <= 151ms` – Half of the requests completed in under 151ms.
* `90% <= 530ms` – 9 out of 10 were faster than 530ms.
* `100% = 530ms` – Longest single request.

---

## Credits

This project is a hands-on assignment from the [Docker Mastery Course](https://www.udemy.com/course/docker-mastery/) by Bret Fisher, aimed at reinforcing Docker fundamentals through real-world examples.

---
