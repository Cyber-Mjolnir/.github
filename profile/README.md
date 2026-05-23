## ⚡ Welcome to Cyber-Mjolnir

Cyber-Mjolnir is a collaborative initiative focused on building high-integrity, privacy-preserving, and cryptographically resilient software frameworks. Our core focus centers on exploring zero-trust ecosystems, advanced decentralized models, and robust implementations of modern cryptographic primitives. 

We are excited to share this project with the global open-source community! Whether you are a security researcher, a cryptography enthusiast, or a software engineer, **we openly welcome your insights, feedback, and contributions.** Feel free to explore the codebase, open issues, fork the repository, and submit pull requests as we continue to benchmark, iterate, and enhance this security framework.

---

# 🏛️ Project Overview
## CSePS: Cryptographically Secure Government e-Procurement System Demo

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/)
[![Security: TNO Architecture](https://img.shields.io/badge/Security-Trust--No--One-red.svg)]()

CSePS is a high-security, distributed e-procurement prototype designed to enforce absolute bidder anonymity, rigorous bid integrity, and binding non-repudiation. By leveraging advanced cryptographic primitives, the system eliminates classic systemic vulnerabilities in public procurement, including insider trading, premature bid disclosure, and unauthorized database tampering.

---

## 🏛️ Core Security Architecture & Cryptographic Foundations

CSePS operates on a strict **Trust-No-One (TNO)** architectural paradigm. Neither the central server, system administrators, nor individual evaluation officers possess the capability to decrypt sensitive bid assets until an explicit, multi-party cryptographic consensus is achieved.



### 1. Advanced Cryptographic Engineering
* **Asymmetric Encryption:** Built entirely upon Elliptic Curve Cryptography using the **SECP256k1** curve (the cryptographic standard powering Bitcoin and Ethereum) for low-overhead, high-security identity management and digital signatures.
* **Data-at-Rest Protection:** Private keys are never committed to permanent storage in plaintext. They are structured utilizing standard **PKCS#8** serialization and encrypted via **AES-256-CBC**, deriving the encryption key dynamically from the user's passphrase via robust key derivation functions.
* **Transport Security (ECIES):** All network payloads are wrapped in the **Elliptic Curve Integrated Encryption Scheme (ECIES)**. Every distinct request/response transaction uses ephemeral **ECDH (Elliptic Curve Diffie-Hellman)** session keys paired with **HKDF** and **AES-GCM**, ensuring strict **Forward Secrecy**.

### 2. Dual-Consensus Threshold Model
Bid safety is enforced by a segmented "Master Key" distributed using **Shamir's Secret Sharing (SSS)**. To initiate a tender opening phase, a strict cross-role threshold must be satisfied:
* **Bidder Threshold:** More than **50% of active participants** must explicitly release their cryptographic decryption shares.
* **Officer Committee:** A **2/3 qualified majority** of the assigned evaluation board must independently approve and dispatch their respective shares.
* **Reconstruction Barrier:** The master decryption key is reconstructed strictly within volatile memory space only when *both* independent thresholds are satisfied simultaneously, neutralizing any single point of failure (SNDF).

### 3. Absolute Privacy & Verifiability
* **Zero-Knowledge Proofs (ZKP):** Bidders provide automated cryptographic range proofs (leveraging Pedersen-style commitments). This allows the system to verify that a submitted bid resides within a tender's valid boundary criteria *without* exposing the exact financial valuation prior to opening.
* **Immutable Blockchain Ledger:** Tenders and bidding steps are saved to a locally chained cryptographic ledger. Blocks are bound together sequentially via high-order hashing, containing preceding block hashes, distinct bidder digital signatures, and ZK proofs for an unalterable audit trail.
* **Self-Healing State Machines:** Upon system initialization, the primary engine triggers a state verification routine across the ledger. If unauthorized file modifications are detected, the system automatically prunes corrupt blocks and requests state synchronization to restore ledger integrity.

---

## 🏗️ System Architecture & Component Mapping

The implementation is decoupled into four primary functional runtime components:

### 🖥️ Primary & Backup Infrastructure Core
* Manages database synchronization, tracks cryptographic state transformations, and maintains the ledger.
* **Integrity Locking:** JSON databases are continuously signed utilizing **HMAC-SHA256** signatures reinforced with a secret system pepper, rendering external manual database manipulations immediately apparent to the self-healing scanner.
* Provides structured **Role-Based Access Control (RBAC)** defining strict system perimeters for Admins, Officers, and Bidders.

### ⚡ Admin Command Console
* **Officer Provisioning:** Emits one-time, cryptographically signed invitation tokens to limit unauthorized enrollment.
* **Identity Pinning:** Validates and explicitly pins Officer public keys to the ledger state machine to block impersonation attacks.
* Provides comprehensive administrative chain-of-custody tracking.

### 📋 Evaluation Officer Portal
* Handles tender instantiation alongside structural definition parameters (e.g., configuring evaluation committee bounds).
* Facilitates threshold release operations post-deadline.
* Executes automated cryptographic matrix evaluations to index winning bid entries instantly once consensus is fully resolved.

### 🆕 Client-Side Bidder Application
* Generates E2EE (End-to-End Encrypted) bid payloads and structural ZKPs locally prior to network transmission.
* Empowers bidders with consensus veto authority, allowing participants to stall decryption vectors if administrative fraud is suspected.
* Features an integrated cryptographic blockchain explorer to let users independently audit public ledger blocks.

---

## 🚀 Installation & Local Environment Setup

### 1. Prerequisites
* **Python 3.8+**
* Git

### 2. Environment Deployment

Clone the repository directly from the **Cyber-Mjolnir** organization workspace:

```bash
# Clone the repository
git clone [https://github.com/Cyber-Mjolnir/ECC-Implementation-Project.git](https://github.com/Cyber-Mjolnir/ECC-Implementation-Project.git)
cd ECC-Implementation-Project

# Instantiate a clean virtual isolation environment
python -m venv venv

# Activate the virtual environment
# On Linux/macOS:
source venv/bin/activate  
# On Windows:
venv\Scripts\activate     

# Install required dependencies
pip install -r requirements.txt
