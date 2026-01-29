# Iraquin Protocol
## A Execution Agnostic High Performance Layer 1


---

## Abstract

Modern blockchains tightly couple execution, ordering, and consensus. This coupling forces all participants to redundantly execute the same computation, limits throughput, and makes correctness dependent on synchronous execution assumptions.

Iraquin introduces a different design point: **execution may be external and fallible, but verification must be deterministic and final**. Iraquin is a Layer‑1 protocol that forms consensus over *verified state transitions*, not over execution itself.

The protocol separates block production and execution (Prime) from asynchronous verification and correction (Omega). Computation— including AI inference and specialized workloads— occurs off‑chain in arbitrary environments, while the blockchain natively verifies proofs, attestations, and state diffs. Incorrect execution is handled through forward correction rather than chain reorganization.

This whitepaper presents the motivation, architecture, and economic model of Iraquin, a verification‑first blockchain designed to scale with computation without replicating it.

---

## 1. Introduction

Blockchains were originally designed to be deterministic replicated state machines: every validator executes every transaction in the same order. While this model simplifies correctness, it imposes fundamental scalability limits and makes general‑purpose execution the dominant cost of consensus.

As computation becomes more heterogeneous— encompassing AI inference, zero‑knowledge proofs, and specialized hardware— forcing all validators to execute all computation becomes both impractical and unnecessary.

Iraquin starts from a different premise:

> **Consensus should decide what is correct, not how computation is performed.**

The protocol decouples execution from verification. Execution can occur anywhere, on any hardware, under any runtime. The blockchain’s role is to verify results, settle state changes, and enforce economic accountability when execution is incorrect.

---

## 2. Design Goals

Iraquin is designed around the following principles:

- **Verification‑first consensus**: state transitions are accepted only after verification
- **No chain reorganization**: incorrect execution is handled via forward correction
- **Execution agnosticism**: no on‑chain VM or fixed runtime
- **Parallelism without sharding**: independent state transitions execute concurrently
- **Asynchronous correctness**: verification need not block liveness
- **Long‑term cryptographic durability**: support for post‑quantum primitives

---

## 3. High‑Level Architecture

Iraquin consists of two tightly coupled protocols:

- **Prime**: optimistic block production and execution coordination
- **Omega**: asynchronous verification, correction, and finality

Additional first‑class components include:

- A consensus‑backed data availability layer
- A native compute and AI job settlement mechanism
- Verifiable governance inputs

At a high level, Prime provides liveness and throughput, while Omega enforces safety and correctness.

---

## 4. Network Roles

### 4.1 Ingress Nodes

Ingress nodes accept transactions and compute job submissions from users. They shard and stream requests into the Prime layer without performing execution or verification.

### 4.2 Miners (Prime)

Miners perform cryptographic verification only. They validate transaction signatures, proofs, and commitments, but do not execute state transitions.

Miners compete within shards using verifiable delay functions (VDFs) to probabilistically select candidate blocks.

### 4.3 Colliders (Prime)

Colliders consume verified miner outputs and perform execution. They merge transactions, partition independent state updates, and compute state diffs.

Colliders are economically accountable for correctness and are slashed for invalid state transitions.

### 4.4 Validators (Omega)

Validators asynchronously re‑verify finalized Prime blocks. They replay state transitions, validate proofs, and detect invalid execution.

When inconsistencies are found, validators issue deterministic correction transactions that restore canonical state.

---

## 5. Prime Protocol: Optimistic Execution

Prime is responsible for throughput and liveness. Transactions are optimistically accepted and executed without waiting for full verification.

Blocks produced by Prime are immediately visible to the network and applications, enabling low‑latency state access.

Importantly, Prime never reorders or reorgs blocks. Incorrect execution is handled exclusively by Omega.

---

## 6. Omega Protocol: Verification and Correction

Omega enforces correctness after execution.

Validators independently re‑execute or verify submitted state transitions. If a block or transaction is found invalid, Omega produces a correction transaction that nullifies incorrect effects and downstream dependencies.

This forward‑correction model avoids rollback while preserving deterministic finality after a bounded dispute window.

---

## 7. Forward Correction Model

Unlike traditional blockchains that rely on reorganization, Iraquin treats incorrect execution as a recoverable fault.

Corrections:
- Are deterministic
- Are economically enforced
- Do not invalidate unrelated state

This model allows the system to remain live even under partial faults or adversarial execution.

---

## 8. Execution Model

Iraquin does not provide an on‑chain virtual machine.

Computation occurs off‑chain in arbitrary environments, including:
- Native binaries
- AI inference runtimes
- Trusted execution environments (TEEs)
- OPML or zero‑knowledge systems

The blockchain verifies proofs and state diffs rather than executing programs.

---

## 9. Data Availability

All transactions, compute inputs, and outputs are committed to a consensus‑backed data availability layer.

Data commitments include hashes, retention periods, and storage fees. Data may be pruned after expiration while remaining verifiable.

---

## 10. Economic Model (Overview)

The native token is used for:
- Ingress, Miner and collider selection
- Validator staking
- Compute job collateral
- Transaction, Storage and Compute fees

Slashing applies to invalid execution, equivocation, and failure to deliver promised computation.

---

## 11. Governance

Governance decisions may be informed by verifiable AI models, but enforcement remains human‑verifiable and protocol‑controlled.

All governance actions are subject to verification and dispute through Omega.

---

## 12. Security Considerations

Iraquin assumes a bounded fraction of adversarial validators.

Safety is enforced via asynchronous verification and economic penalties. Liveness is maintained through optimistic execution.

---

## 13. Conclusion

Iraquin reframes the role of a blockchain from an execution engine to a verification and settlement layer for computation.

By decoupling execution from consensus, the protocol enables scalable, heterogeneous computation while preserving deterministic correctness.

---

*End of Draft Whitepaper*

