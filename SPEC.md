# Iraquin Blockchain

## End-to-End Protocol Specification (Draft)

---

## 1. Design Goals

Iraquin is a Layer-1 blockchain designed around the following core goals:

* High-throughput execution without duplicate work
* Deterministic correctness without reorgs
* Native support for verifying decentralized compute and AI inference
* Parallelism without state sharding
* Optimistic liveness with accountable correction
* Long-lived cryptographic durability

The protocol prioritizes **verified state transitions** over repeated execution.

---

## 2. High-Level Architecture

Iraquin consists of two tightly coupled protocols:

* **Prime** — optimistic, high-throughput block production
* **Omega** — asynchronous verification, correction, and finality

Additional first-class components:

* Consensus-backed Data Availability (DA) Layer
* Native Compute & AI Marketplace
* Verifiable AI-assisted Governance
* Post-quantum cryptographic primitives

---

## 3. Network Roles

### 3.1 Miners

Miners perform **cryptographic verification only**.

They verify:

* Transaction signatures
* Proofs
* Commitments

They **do not**:

* Execute state
* Determine ordering

Properties:

* Compete within miner shards using VDF-based selection
* Output **intermediate miner blocks**

---

### 3.2 Colliders

Colliders are responsible for **execution and state correctness**.

They:

* Consume verified miner blocks
* Execute transactions
* Update state
* Assemble final blocks

Responsibilities:

* State correctness
* Transaction grouping

Penalties:

* Slashed for invalid state transitions

---

### 3.3 Validators (Omega)

Validators operate in the **Omega protocol**.

They:

* Asynchronously re-verify finalized blocks
* Detect:

  * Invalid execution
  * Double commitments
  * Inconsistent state transitions

They:

* Issue **forward correction transactions**
* Control final reward settlement

---

### 3.4 Ingress Nodes

Ingress nodes:

* Bridge users and the miner pool
* Stream transactions to miner shards

---

### 3.5 Compute Workers

Compute workers:

* Execute compute or AI jobs off-chain

They operate inside:

* Trusted Execution Environments (TEEs)
* OPML-compatible environments

Requirements:

* Stake collateral for job commitments

---

## 4. Epoch & Slot Structure

* Time is divided into **epochs**
* Each epoch contains a fixed number of **slots**

Elections determine:

* Miner sets
* Collider(s) for a future epoch

Properties:

* Epochs are time-bound
* Slot ownership is probabilistic via VDF
* Leadership rotates deterministically

---

## 5. Prime Protocol (Block Production)

### 5.1 Transaction Flow

1. User submits transaction → ingress nodes
2. Ingress nodes shard and stream transactions
3. Miners verify signatures & proofs
4. Miners produce candidate blocks per shard
5. VDF selects winning miner block per shard
6. Colliders consume winning blocks

---

### 5.2 Miner Blocks

Miner blocks contain:

* Verified transaction IDs
* Proof metadata

Miner blocks **do not** contain:

* State transitions
* Execution results

---

### 5.3 Collider Execution

Colliders:

* Merge miner outputs
* Partition transactions by disjoint account sets
* Execute transactions in-memory

Parallelism:

* Multiple colliders may produce parallel blocks
* Blocks may form a **DAG per epoch**

---

### 5.4 Optimistic Finality

* Collider blocks are optimistically accepted
* Blocks are broadcast immediately
* No synchronous validator approval
* No reorgs at the Prime layer

---

## 6. Omega Protocol (Verification & Finality)

### 6.1 Asynchronous Verification

Validators:

* Replay state transitions
* Verify execution post block production

They detect:

* Invalid execution
* Inconsistent commitments

---

### 6.2 Forward Correction Model

* No chain rollback
* If invalid state is found:

  * Omega issues a correction transaction
  * Invalid effects and downstream state are nullified

Corrections are:

* Deterministic
* Economically enforced

---

### 6.3 Dispute Window

* Blocks remain reversible for a bounded window
* Supports:

  * Delayed OPML verification
  * TEE attestation checks

After window expiry:

* State becomes irreversible

---

### 6.4 Reward Settlement

* Rewards finalized only after Omega approval
* Invalid work results in:

  * Slashing
  * Forfeited rewards

---

## 7. Data Availability (DA) Layer

* DA is part of consensus

All compute jobs, AI inputs, and results:

* Are committed via the DA layer

DA commitments include:

* Data hash
* TTL
* Storage fee

Data lifecycle:

* Pruned after TTL
* Archived by archival nodes

---

## 8. Compute & AI Marketplace

### 8.1 Job Lifecycle

1. Job submitted to DA layer
2. Job enters auction
3. Workers bid with:

   * Price
   * Completion deadline
   * Collateral stake
4. Worker executes job
5. Result submitted with proof
6. Miners verify proof
7. State updated upon verification

---

### 8.2 Failure Handling

* Missed deadline → stake slashed
* Invalid proof → transaction dropped
* Job re-enters auction automatically

---

## 9. Execution Model

* No on-chain VM
* No EVM

Contracts execute:

* Off-chain
* In any language
* Inside TEEs

Blockchain verifies:

* State diffs
* Execution proofs

Only **verified state transitions** are committed.

---

## 10. Verifiable AI Governance

AI models:

* Propose protocol parameters
* Do not enforce them

All AI outputs are:

* Verifiable
* Challengeable

Governance decisions include:

* Fee parameters
* Sharding thresholds
* Burn rates
* Resource allocation

Validators enforce changes only after proof validation.

---

## 11. Cryptography

* Post-quantum signatures: **Dilithium**
* Merklized commitments for:

  * Blocks
  * Miner outputs
  * State diffs
* VDFs for:

  * Miner selection
  * Fairness enforcement

---

## 12. Token Economics (High-Level)

Token is used for:

* Miner, collider, ingress burn-based elections
* Validator staking
* Compute job collateral
* Transaction fees

Slashing applies to:

* Invalid execution
* Equivocation
* Failure to deliver compute

---

## 13. Security Model

* Assumes < quorum adversarial validators
* Liveness via optimistic execution
* Safety via:

  * Asynchronous verification
  * Forward correction

No MEV-style ordering games due to:

* Unpredictable transaction partitioning

---

## 14. Key Properties

* No reorgs
* No sharding
* No duplicate execution
* Parallel block production
* Compute-first economics
* Verifiable AI-native governance

---

## 15. Scope of Whitepaper vs Yellow Paper

This specification supports:

### Whitepaper

* Design
* Motivation
* Architecture
* Economics

### Yellow Paper

* Formalization of:

  * Prime selection
  * Omega correction rules
  * DAG consistency
  * Slashing conditions

---

*End of Draft Specification*
