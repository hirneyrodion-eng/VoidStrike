<p align="center">
  <pre align="center">
  ██╗    ██╗ ██████╗ ██╗██████╗
  ██║    ██║██╔═══██╗██║██╔══██╗
  ██║    ██║██║   ██║██║██║  ██║
  ╚██╗ ██╔╝██║   ██║██║██║  ██║
   ╚████╔╝ ╚██████╔╝██║██████╔╝
    ╚═══╝   ╚═════╝ ╚═╝╚═════╝
  </pre>
</p>

<p align="center">
  <em>Asynchronous CLI utility for load-testing HTTP services (Layer 7)</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue.svg" alt="Python Version">
  <img src="https://img.shields.io/badge/Engine-asyncio%20%2F%20aiohttp-orange.svg" alt="Async Engine">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
</p>

---

## ⚠️ Purpose & Responsible Use

**VoidStrike** is an educational pet project for learning asynchronous programming in Python and web-service load-testing techniques. The tool is intended **exclusively** for measuring the performance and resilience of infrastructure that you own, or that you have explicit written permission to test.

> Running any load-testing tool against systems you do not own, without authorization, may violate the law (e.g. the CFAA and equivalents) and your provider's terms of service. The author assumes no responsibility for misuse. Use it only in isolated or self-owned environments (localhost, a staging stand, a dev environment).

---

## ⚡ Overview

The utility is built on `asyncio` and `aiohttp` and lets you generate controlled HTTP load against your own service while collecting metrics on its behavior: response codes, response time, and the stability of response sizes under load. This helps locate bottlenecks, tune limits, and observe how a service degrades as the number of concurrent requests grows.

---

## ⚙️ Features

* **Asynchronous engine** — high throughput with low resource usage thanks to a single event loop (`asyncio` + `aiohttp`).
* **Flexible port specification (Nmap-style)** — single ports, lists, and ranges: `-p 80,443,8000-8010`.
* **Load control** — configurable concurrency (`-c`) and total number of requests per port (`-n`).
* **Metrics collection** — counts of response codes (`200`, `429`, `403`, errors), total run time, and average throughput (requests/sec).
* **Response analysis** — tracking of the number of unique response sizes (`Content-Length`) and a warning about anomalies when the service's responses start to "drift" under load.
* **Readable output** — a colored per-target report in the terminal.

---

## 📦 Requirements & Installation

```bash
# Python 3.10+
git clone <repo-url>
cd voidstrike

python3 -m venv .venv
source .venv/bin/activate

pip install aiohttp
```

---

## 🕹️ Usage

```bash
python3 DoS.attack -u <URL> -p <PORTS> -c <CONCURRENCY> -n <REQUESTS>
```

### Arguments

| Flag | Description | Default |
|------|-------------|---------|
| `-u`, `--url` | Base URL or IP of the target (e.g. `http://127.0.0.1`) | — (required) |
| `-p`, `--ports` | Nmap-style ports: `80`, `80,443`, `8000-8010` | `80` |
| `-c`, `--concurrency` | Number of simultaneous requests | `50` |
| `-n`, `--requests` | Number of requests per port | `200` |

### Example (local test server)

Spin up a simple service to experiment with and test it:

```bash
# Terminal 1 — test server on localhost
python3 -m http.server 8080

# Terminal 2 — load against your own server
python3 DoS.attack -u http://127.0.0.1 -p 8080 -c 50 -n 500
```

### Sample output

```
[*] Target: http://127.0.0.1:8080/
[*] Concurrency: 50 | Requests: 500
[✓] Success (200): 500 | [!] Rate Limit (429/403): 0 | [✗] Errors: 0
[i] Content analysis: Unique response sizes: 1 | Avg size: 1024 bytes
[-] Time: 1.84 sec
[⚡] Throughput: 271.74 req/sec
```

---

## 📊 Reading the Metrics

* **Success (200)** — the service is handling requests normally.
* **Rate Limit (429/403)** — limits/protection kicked in; useful for verifying your own limits.
* **Errors** — timeouts and connection failures; growth under load points to resource exhaustion.
* **Unique response sizes > 1** — responses change under load (e.g. partial responses or error pages), a reason to investigate degradation.

---

## 🛠️ Tech Stack

* **Python 3.10+**
* **asyncio** — asynchronous event loop
* **aiohttp** — asynchronous HTTP client
* **argparse** — command-line argument parsing

---

## 🗺️ Possible Improvements

* Export metrics to JSON/CSV for further analysis.
* Response-time percentiles (p50/p95/p99).
* Load charts and run-to-run comparisons.
* Support for custom HTTP methods and request bodies.

---

## 📄 License

The project is distributed under the MIT License. Use it responsibly.

---

## Disclaimer

This tool was developed exclusively for educational purposes, for security research, and for authorized penetration testing of your own web applications and lab environments. The author assumes no responsibility for any misuse or damage caused by this software. Use it legally and ethically.
