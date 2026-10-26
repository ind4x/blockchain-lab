# TP 02: Proof of Stake Protocol & Network Slashing

This laboratory implements a distributed Proof of Stake (PoS) consensus network.
The architecture models validator registration, stake-weighted proposer selection,
deterministic block verification, economic reward compounding, and punitive slashing
mechanisms over a simulated peer-to-peer network managed by a central Flask REST server.

---

## Lab Structure

```text
TP-02/
├── README.md                          # Laboratory documentation and architecture specification
├── Server.py                          # Base Flask network server defining state, data models, and REST endpoints
├── run_server.py                      # Server runner injecting functional consensus logic (R2, R5, R6, R7)
├── main.py                            # Headless test runner orchestrating full network lifecycle and stress tests
├── main.ipynb                         # Interactive notebook stepping through protocol rules and validation
├── participants/                      # Distributed participant client environments
│   ├── Participant-1.ipynb            # Validator node: Idriss (Stake: 40 ETH)
│   ├── Participant-2.ipynb            # Validator node: Nada (Stake: 30 ETH)
│   ├── Participant-3.ipynb            # Validator node: Ayoub (Stake: 20 ETH)
│   └── Participant-4.ipynb            # Validator node: Youssef (Stake: 10 ETH)
└── docs/
    ├── TP02_PoS_new.pdf               # Official assignment guidelines and protocol rules
    ├── rapport_02_blockchain.pdf      # Detailed analytical report and experimental validation
    └── rapport_02_blockchain.tex      # LaTeX source for the formal laboratory report
```

### File Responsibilities

- **`Server.py`**: Declares global state containers (`validateurs`, `chaine`), constants (`MISE_MINIMALE = 10`, `GRAINE = 2`), and REST API endpoints (`/rejoindre`, `/validateurs`, `/chaine`, `/produire_bloc`, `/verifier`, `/falsifier`, `/sanctionner`).
- **`run_server.py`**: Clean runner injecting pure mathematical implementations of proposer selection, rewards, block verification, and stake slashing into `Server.py`.
- **`main.py`**: Automated end-to-end simulation executing Steps 1 to 10 (Rules R1 to R7) and running 4 rigorous network verification scenarios.
- **`main.ipynb`**: Interactive notebook providing visual inspection, table summaries, and statistical convergence charts.
- **`participants/Participant-[1-4].ipynb`**: Individual client notebooks simulating independent distributed nodes connecting to the local HTTP node.

---

## Architecture & Consensus Protocol

### Network Topology
The system implements a client-server architecture mimicking a blockchain node communication layer:

```text
       +---------------------------------------------+
       |           Flask Central Node                |
       |  - Staking Registry (validateurs)           |
       |  - Ledger History (chaine)                  |
       |  - Verifiable Random Engine (R2 / R4)       |
       +---------------------------------------------+
               ^             ^             ^
               |             |             |
        HTTP   |      HTTP   |      HTTP   |
               v             v             v
       +-------------+ +-------------+ +-------------+
       | Participant | | Participant | | Participant |
       | 1 (Idriss)  | |  2 (Nada)   | | 3 (Ayoub)   |
       | Stake: 40   | | Stake: 30   | | Stake: 20   |
       +-------------+ +-------------+ +-------------+
```

### Consensus Rules (R1 - R7)

1. **R1 (Minimum Stake):** Nodes must deposit a stake $m_i \ge 10\text{ ETH}$ to register as active validators. Submissions below the threshold return HTTP 400.
2. **R2 (Stake-Weighted Proposer Selection):** Total network stake is defined as $M = \sum_{j} m_j$. Each validator occupies an interval $[S_{i-1}, S_i]$ on $[0, M]$ of length equal to their stake. Given a verifiable draw $T \in [0, M]$, the elected proposer $i^*$ satisfies:
   $$S_{i^*-1} < T \le S_{i^*}$$
3. **R3 (Single Block Proposer):** Each block is proposed by exactly one validator elected by rule R2.
4. **R4 (Deterministic Verifiability):** Each block commits to the exact snapshot of stakes `mises` and the draw `tirage`. Any node can independently replay `choisir_proposant(mises, tirage)` to verify the proposer's legitimacy.
5. **R5 (Block Rewards & Compounding):** Successful block production increments the validator's block count and compounds their stake by reward rate $\tau$:
   $$m_i \leftarrow m_i \cdot (1 + \tau)$$
6. **R6 (Fraud Detection & Invariant Verification):**
   - **Deterministic Check:** Confirms that `bloc["proposant"] == choisir_proposant(bloc["mises"], bloc["tirage"])`.
   - **Statistical Check:** Flags any node whose observed block production share exceeds their stake percentage by more than 3%:
     $$\frac{\text{Blocks}_i}{|\text{Chain}|} - \frac{m_i}{M} > 0.03$$
7. **R7 (Slashing & Network Exclusion):** When fraud or Byzantine behavior is verified, the `/sanctionner` endpoint confiscates a fraction $\alpha = 0.5$ of the malicious validator's stake and permanently sets `exclu = True`, barring them from all future draws.

---

## Execution Instructions

### 1. Prerequisites and Dependencies
Ensure Flask and Requests are installed:

```bash
pip install flask requests
```

### 2. Option A: Headless Automated Simulation
Run the complete simulation script (which automatically spins up the server in a background thread if not already running):

```bash
cd TP-02
python3 main.py
```

### 3. Option B: Interactive Execution via Jupyter Notebook
1. Start the Flask server in a terminal:
   ```bash
   cd TP-02
   python3 run_server.py
   ```
2. Open Jupyter in a separate terminal:
   ```bash
   jupyter notebook main.ipynb
   ```
3. Open and run `participants/Participant-1.ipynb` through `Participant-4.ipynb` to simulate multi-party participation.

---

## Technology Stack

- **Language:** Python 3
- **Network Framework:** Flask (RESTful HTTP microframework)
- **Client Networking:** Requests
- **Environment:** Jupyter Notebook, VS Code

---

## Author

- **Author:** Youssef Fellah ([ind4x_](https://github.com/ind4x))
- **Institution:** Mundiapolis University, Engineering Cycle
