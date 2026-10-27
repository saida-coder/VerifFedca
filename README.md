# 🛡️ VeriFedFCA
## Formal Verification and Security Auditing for Federated Formal Concept Analysis

<p align="center">
  <img src="verifedfca-mark.svg" alt="VeriFedFCA logo" width="900" />
</p>

<p align="center">
  <img alt="Federated Formal Concept Analysis" src="https://img.shields.io/badge/Federated-Formal%20Concept%20Analysis-087e8b?style=for-the-badge" />
  <img alt="Authenticated exchange" src="https://img.shields.io/badge/Security-Authenticated%20Messages-165d77?style=for-the-badge&logo=letsencrypt&logoColor=white" />
  <img alt="Open research" src="https://img.shields.io/badge/Research-Reproducibility-19548a?style=for-the-badge&logo=github&logoColor=white" />
</p>

**VeriFedFCA** is a security-aware and privacy-preserving **Federated Formal Concept Analysis (FedFCA)** research framework designed for trustworthy distributed concept-lattice construction.

It enables multiple providers to collaborate on the construction of a global concept lattice **without sharing their raw local formal contexts**, while adding validation to protect the aggregation process against modified, replayed, unauthorized, or otherwise invalid protected lattice messages.

VeriFedFCA extends the [FedFCA framework](https://doi.org/10.1007/s10586-025-05778-z) with an authenticated communication and protected-message validation layer. The research also defines formal protocol-verification objectives and implementation security-auditing activities to analyze security properties beyond data confidentiality.

> **Research scope:** authentication and integrity checks do not guarantee that the semantic content supplied by an authorized participant is truthful. Formal verification and Valgrind auditing are *planned components in the supplied manuscript* unless corresponding models and reports are released. Experimental claims should be traceable to actual code and logs.

---

# 🌐 Global Architecture

![VeriFedFCA Global Architecture](assets/global_architecture.svg)

The architecture consists of distributed **federated providers**, a protected communication channel, and a **validation-aware aggregation server**. Only accepted contributions enter the global lattice construction process.

---

# 👥 Federated Participants

Each provider performs local operations including:

- 📂 Constructing a local formal context
- 🔒 Applying privacy-preserving processing, including Randomized Response in the proposed configuration
- 🧠 Generating a local formal concept lattice
- 🔐 Protecting serialized lattice messages with cryptographic mechanisms
- ✍️ Authenticating contributions using digital signatures
- 📡 Transmitting protected messages to the aggregation server

Local source records remain with their respective data providers. The privacy mechanism and transmitted representation must still be evaluated for potential information leakage.

---

# 🖥️ Validation-Aware Aggregation Server

The aggregation server is responsible for:

- 🪪 Checking provider identity and digital signatures
- 🔏 Verifying protected-message integrity
- ⏱️ Detecting replayed or stale messages
- 🧾 Checking message validity and handling malformed payloads
- 🚫 Rejecting protected lattice messages that fail validation
- 📝 Recording security-validation outcomes
- 🌐 Aggregating accepted local conceptual representations
- 🧩 Constructing the resulting global concept lattice

The central rule is **validate before aggregation**. Rejected messages must not contribute to the verified global lattice.

---

# ✨ Main Features

- 🔒 Privacy-aware Federated Formal Concept Analysis
- 🛡️ Authenticated protected lattice message exchange
- ✅ Digital-signature verification
- 🔐 Ciphertext integrity checks
- ⏱️ Freshness and replay-attack detection
- 🚫 Validation-aware contribution rejection
- 📝 Security-event tracking and detection analysis
- 🌍 Aggregation of accepted lattice contributions
- 📏 Computational and communication overhead evaluation
- 🧩 Local and global conceptual-similarity evaluation objectives
- 🧪 Formal protocol-verification objectives
- 🔍 Implementation-level security-auditing objectives
- ⚡ Evaluation across IID and Non-IID federations

---

# ⚠️ Threat Model

VeriFedFCA considers attacks against protected lattice messages and their contribution to the global result:

- ❌ **Message tampering:** unauthorized modification of transmitted content
- 🔁 **Replay attacks:** resubmission of previously valid messages
- ✍️ **Invalid signatures:** forged or unauthenticated messages
- 🧨 **Corrupted ciphertext:** protected payloads with invalid integrity checks
- 🚷 **Unauthorized participation:** attempts to submit updates without recognized credentials
- ⚙️ **Malformed messages:** invalid serialization or missing required fields

The main objective is to detect and reject invalid messages before they affect global aggregation.

**Limitation:** a malicious provider may send a correctly signed but semantically misleading local lattice. Cryptographic validity alone does not establish FCA structural correctness, support consistency, or resistance to collusion. Those properties require distinct defenses and experiments.

---

# 🧠 Protected Lattice Message Validation Layer

The proposed validation layer brings together complementary checks.

## 🪪 Signature and Provider Authentication

- ✅ Validate the sender's digital signature
- 🔑 Associate the signing key with an authorized provider
- 🚫 Reject unknown or invalid signatures

The manuscript specifies **Ed25519** for digital signatures.

---

## 🔐 Message Integrity Verification

- 🧾 Validate the integrity of the received encrypted payload
- 🔓 Perform authenticated decryption where required
- 🚫 Reject modified or corrupted ciphertext

The proposed cryptographic configuration includes **AES-256-GCM** and **SHA-256**. Their precise use and key-handling procedures must match the committed implementation.

---

## ⏱️ Freshness and Replay Protection

- 🕒 Check message freshness information
- 🔁 Detect duplicate or replayed submissions
- 🚫 Exclude stale contributions from aggregation

The actual implementation should specify its nonce, sequence-number, or timestamp policy and permitted replay window.

---

## ✅ Validation-Aware Aggregation

- 📥 Receive protected local lattice messages
- 🛡️ Apply the configured validation checks
- ✅ Accept contributions that pass validation
- ❌ Reject and log invalid contributions
- 🌐 Build the global lattice from the accepted set

This mechanism is separate from semantic validation of the provider's conceptual content.

---

# 🔎 Formal Verification and Security Auditing

## 🧪 Formal Protocol Verification

The research design considers **ProVerif** to analyze formally specified security properties, such as confidentiality and authentication. A completed formal-verification claim requires the protocol model, security queries, and actual tool output.

## 🛠️ Security Auditing

The research design considers **Valgrind/Memcheck** for implementation-level defect analysis where the implementation language and runtime make it applicable. A completed audit claim requires the executed commands and inspection reports.

These two assurance activities are not substitutes for the runtime message-validation checks.

---

# 📚 Baselines

The evaluation methodology compares the following approaches:

| Method | Description |
|:--|:--|
| Centralized FCA | Reference concept-lattice construction for structural comparison |
| FedFCA | Federated FCA baseline |
| Secure FedFCA | Federated baseline with partial protected-message security validation |
| 🛡️ **VeriFedFCA** | Proposed verification-aware and validation-aware framework |

The baselines listed as experimentally compared should correspond to implementations and execution logs in the repository.

---

# 📂 Benchmark Datasets

## ♟️ Chess (King-Rook vs. King-Pawn)

- 📦 Objects: **3,196**
- 🏷️ Original features: **35**
- 🔗 Source: [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/22/chess%2Bking%2Brook%2Bvs%2Bking%2Bpawn)

Categorical chess-position benchmark used for concept extraction and lattice analysis.

---

## 🍄 Mushroom

- 📦 Objects: **8,124**
- 🏷️ Original features: **22**
- 🔗 Source: [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/73/mushroom)

Categorical benchmark with rich attribute-value combinations. The number of binary FCA attributes may differ after encoding.

---

## 🔗 Record Linkage Comparison Patterns

- 📦 Record pairs: **5,749,132**
- 🏷️ Dataset fields listed in the manuscript: **12**, including identifiers and the matching-status field
- 🔗 Source: [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/210/record%2Blinkage%2Bcomparison%2Bpatterns)

Large record-comparison benchmark for evaluating distributed processing and data-preparation choices.

---

## 🛒 T10I4D100K

- 📦 Transactions: **99,935** in the cited version
- 🏷️ Distinct items: **870**
- 🔗 Source: [Mendeley Data, Version 1](https://doi.org/10.17632/vcmcb3yd2s.1)

Sparse transactional benchmark commonly used in pattern mining. **The benchmark dataset is synthetic, but measurements obtained by actually running the implementations on it are experimental results.**

---

# ⚙️ Experimental Settings

The manuscript specifies the following evaluation configurations:

- 👥 Provider counts: **50, 100, 150, 200, and 250**
- 🌍 Data distributions: **IID** and **quantity-skewed Non-IID**
- 🔒 Privacy budgets (ε): **0.1, 0.3, 0.5, 0.7, and 0.9**
- 🔐 Protected-message operations: **AES-256-GCM, SHA-256, and Ed25519**
- ⚠️ Injected invalid-message categories: **tampering, replay, invalid signature, corrupted ciphertext**
- 🧩 Global aggregation objective: **infimum/supremum-based FCA aggregation**

For reproducibility, record the exact hardware, dependencies, provider-partition seeds, number of repetitions, and parallelization settings used for each result.

---

# 📏 Evaluation Metrics

VeriFedFCA is evaluated using four main metric families:

- ⏱️ **Cumulative Execution Time:** total provider-side processing work; end-to-end wall-clock time must be measured separately
- 📡 **Communication Overhead:** serialized transmitted message sizes, reported in MiB
- 🧩 **Lattice Similarity:** local-lattice similarity and, when both global concept sets are available, global Jaccard similarity
- 🎯 **Invalid-Message Detection Rate:** proportion of injected invalid messages correctly detected

An additional recommended metric is the **false-positive rate**, which measures legitimate messages incorrectly rejected.

---

# 📈 Experimental Results

The experimental analysis is organized around four comparisons:

- ⏱️ **Runtime scalability:** how cumulative processing time changes as federation size increases
- 📡 **Communication costs:** how protected-message sizes change with additional validation metadata
- 🧩 **Conceptual similarity:** how accepted contributions relate to local and global concept-lattice structure
- 🛡️ **Attack detection:** how each framework responds to injected invalid protected messages

The manuscript contains figures for IID and Non-IID configurations across all four datasets. For a reproducible release, the corresponding **raw result tables, execution logs, attack-injection records, and plot-generation scripts** should accompany those figures in this repository.

> **Publication check:** the supplied manuscript labels some existing plots and counts as *synthetic scenario outputs*. Before stating numerical improvements here as measured results, verify that they exactly match real execution logs. Do not relabel scenario-generated values as experimental observations.

---

# 🛠️ Technologies and Methods

- 📚 Formal Concept Analysis (FCA)
- 🌐 Federated concept-lattice construction
- 🔒 Randomized Response for privacy-aware local processing
- 🔐 AES-256-GCM authenticated encryption (specified design)
- ✍️ Ed25519 digital signatures (specified design)
- 🧾 SHA-256 hashing (specified design)
- 🧪 ProVerif (planned formal-analysis tool unless reports are supplied)
- 🔍 Valgrind/Memcheck (planned auditing tool unless reports are supplied)
- 🗂️ GitHub for code, documentation, and reproducibility artifacts

---

# 📖 Citation

If you use **VeriFedFCA** in academic work, cite the corresponding manuscript:

> Saida Sfaxi, Mokhtar Sellami, and Mohamed Mohsen Gammoudi. *VeriFedFCA: A Formal Verification and Security Auditing Framework for Federated Formal Concept Analysis*. Manuscript.

The underlying FedFCA research is described in:

> Sfaxi, S., Sellami, M., and Gammoudi, M. M. *FedFCA: Federated Formal Concept Analysis Framework*. **Cluster Computing** (2026). [https://doi.org/10.1007/s10586-025-05778-z](https://doi.org/10.1007/s10586-025-05778-z).

---

# 👩‍🔬 Authors

- **Saida Sfaxi**, Faculty of Science of Tunis, University of Tunis El Manar, Tunisia
- **Mokhtar Sellami**, FSJEGJ Jendouba / RIADI Lab, Tunisia
- **Mohamed Mohsen Gammoudi**, ISAMM Manouba / RIADI Lab, Tunisia

---

# 🔗 GitHub Repository

The code, experimental data, documentation, and generated figures should be organized and referenced within **this GitHub repository**. Add tested setup and execution commands once the actual source tree is finalized.

Original FedFCA framework: [msellamiTN/fedfca-framework](https://github.com/msellamiTN/fedfca-framework).

> **License:** no software license was specified in the manuscript. Add a `LICENSE` file before advertising reuse rights. Third-party dataset licenses apply independently.

<p align="center">
  <img src="assets/verifedfca-mark.svg" alt="VeriFedFCA shield" width="70" /><br />
  <b>VeriFedFCA</b><br />
  <sub>Protect · Validate · Aggregate</sub>
</p>
