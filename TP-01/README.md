# TP 01: SHA-256 Hashing, Block Chaining, and Proof of Work

This laboratory explores the core foundations of decentralized ledgers:
cryptographic hashing, immutable data structures, Proof of Work (PoW)
consensus, and the mechanics of tamper-evident history.

## Lab Structure

```text
TP-01/
├── README.md                          # Laboratory documentation and technical specification
├── main.ipynb                         # Interactive notebook containing implementations and experiments
└── docs/
    ├── TP01.pdf                       # Official laboratory assignment and guidelines
    └── rapport_01_blockchain.pdf      # Detailed analytical report and theoretical synthesis
```

### File Responsibilities

- **`main.ipynb`**: Complete executable Python implementation covering SHA-256 hashing properties, block object modeling, genesis block creation, cryptographic chaining, difficulty target mining, automated ledger validation, and re-mining tampering cost evaluations.
- **`docs/TP01.pdf`**: Course specification defining requirements, test cases, and cryptographic exercises.
- **`docs/rapport_01_blockchain.pdf`**: Formal engineering report synthesizing results, complexity curves, and security implications of hash-based distributed ledgers.

---

## Architecture & Technical Design

### 1. Block Anatomy and Data Structure
Each block $B_i$ is modeled as an immutable key-value container bound cryptographically to its predecessor:

$$\text{Block}(i) = \{ i, \text{timestamp}, \text{data}, \text{nonce}, H(B_{i-1}), H(B_i) \}$$

```text
+-------------------------------------------------------+
| Block Index: i                                        |
| Timestamp: 1729950000                                 |
| Previous Hash: H(B_{i-1})                             |
| Transactions / Data: [...]                            |
| Nonce: 48291                                          |
+-------------------------------------------------------+
                           |
                           v
                   SHA-256 Hashing
                           |
                           v
| Block Hash: 0000a4f8... (Satisfies target difficulty) |
+-------------------------------------------------------+
```

### 2. Proof of Work Consensus Mechanism
The Proof of Work consensus enforces computational expenditure to achieve state finality:
- **Condition:** Find a `nonce` $\in \mathbb{N}$ such that $H(\text{Block}(i)) < \text{Target}$.
- **Target Difficulty ($k$):** Requires the resulting hexadecimal hash string to begin with $k$ consecutive zeros:
  $$\text{Prefix}(H(B_i), k) == \underbrace{\text{"00...0"}}_{\text{k times}}$$
- **Complexity:** The search space follows a geometric distribution with expectation $\mathbb{E}[\text{hashes}] = 16^k$.

```text
[Input Block Header] ---> Append Nonce (0, 1, 2, ...) ---> SHA-256
                                                               |
                     +-----------------------------------------+
                     v
             Hash starts with k zeros?
             ├── NO  ---> Increment Nonce -> Retry
             └── YES ---> Block Mined -> Append to Ledger
```

### 3. Ledger Validation & Tamper Propagation
The integrity pipeline performs sequential validation across the ledger:
1. **Hash Verification:** Recomputes $H(B_i)$ from block payload and confirms target difficulty compliance.
2. **Link Integrity:** Verifies that $B_i.\text{previous\_hash} == B_{i-1}.\text{hash}$.
3. **Avalanche Propagation:** Any historical modification at block $B_j$ ($j < n$) produces a cascade of invalid hashes $\forall m \ge j$, requiring an attacker to re-mine $n - j + 1$ blocks sequentially against exponential difficulty.

---

## Execution Instructions

### Prerequisites
- Python 3.10+
- Jupyter Notebook / VS Code Jupyter extension

### Running the Notebook
From the repository root or the `TP-01` directory:

```bash
cd TP-01
jupyter notebook main.ipynb
```

Execute all cells sequentially to run hashing benchmarks, PoW difficulty simulations (scaling from difficulty 1 to 5), chain validation routines, and the historical tampering attack demonstration.

---

## Technology Stack

- **Language:** Python 3
- **Environment:** Jupyter Notebook
- **Standard Libraries:** `hashlib`, `time`, `json`
- **Institution:** Mundiapolis University, Engineering Cycle
- **Author:** Youssef Fellah ([ind4x_](https://github.com/ind4x))
