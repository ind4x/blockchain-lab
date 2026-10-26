# Blockchain Laboratory Portfolio

This repository hosts practical implementations, consensus algorithms, and
architectural models developed for the Blockchain Engineering curriculum at
Mundiapolis University.

---

## Laboratory Overview

| Laboratory | Domain & Topic | Core Methodology | Primary Deliverables | Reference |
| :--- | :--- | :--- | :--- | :--- |
| **[TP-01](./TP-01)** | Cryptographic Primitives & Proof of Work | SHA-256 digest properties, block chaining, PoW mining loops, exponential difficulty scaling, tampering cost analysis | Immutability validation, avalanche demonstration | [Notebook](./TP-01/main.ipynb) |
| **[TP-02](./TP-02)** | Proof of Stake & Network Slashing | Flask REST client-server architecture, stake-weighted proposer selection ($R_1-R_7$), compounding staking rewards, statistical fraud detection, slashing | Multi-node simulation, stress test suite | [Notebook](./TP-02/main.ipynb)<br>[Runner](./TP-02/main.py) |

---

## General Running Instructions

### Prerequisites

- **Python:** 3.10 or higher
- **Git**
- **Virtual Environment Tooling:** `venv`

### Setup and Environment

1. **Clone the repository:**
   ```bash
   git clone git@github.com:ind4x/blockchain-lab.git
   cd blockchain-lab
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install jupyter flask requests
   ```

### Executing Laboratories

#### Running TP-01 (Proof of Work)
Launch Jupyter from the repository root:
```bash
jupyter notebook TP-01/main.ipynb
```

#### Running TP-02 (Proof of Stake Simulation)
* **Automated execution:**
  ```bash
  cd TP-02
  python3 main.py
  ```
* **Interactive execution:**
  In one terminal, start the PoS server:
  ```bash
  python3 TP-02/run_server.py
  ```
  In a second terminal, open the notebook:
  ```bash
  jupyter notebook TP-02/main.ipynb
  ```

---

## Technology Stack

- **Language:** Python 3
- **Network Framework:** Flask, Werkzeug
- **HTTP Client:** Requests
- **Cryptography & Data:** `hashlib`, `json`, `math`, `random`
- **Interactive Development:** Jupyter Notebook, VS Code

---

## Author

- **Author:** Youssef Fellah ([ind4x_](https://github.com/ind4x))
- **Institution:** Mundiapolis University, Engineering Cycle
