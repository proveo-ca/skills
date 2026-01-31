# Cost Analysis: Registry & Storage

This document outlines the cost structure for the `proveo/skills` architecture.
We assume a standard "Skill" payload (JSON/Text) is approximately **100KB**.

## 1. Control Variables: Web2 (Centralized)
*The baseline for performance and cost. Need strategies to avoid single point of failure.*

### A. AWS S3 (Standard)
*   **Storage:** $0.023 per GB / Month.
*   **Bandwidth:** Expensive (~$0.09/GB).
*   **Verdict:** Industry standard, but expensive bandwidth.

### B. Digital Ocean Spaces (S3 Compatible)
*   **Storage:** $5.00 / Month (includes 250GB). Effective: **$0.02 per GB**.
*   **Bandwidth:** 1TB included.
*   **Verdict:** Better value than AWS for high-traffic reading.

### C. Hetzner Storage Box (The Floor)
*   **Storage:** ~€3.81 / Month (1 TB). Effective: **~$0.004 per GB**.
*   **Verdict:** The absolute cheapest option.
*   **Cost per 100KB Skill:** $0.0000004 / month. (Negligible).

---

## 2. The Storage Layer (Web3 Options)

### Option A: IPFS (Managed via Pinata)
*   **Storage:** ~$0.15 per GB / Month.
*   **Verdict:** **37x more expensive** than Hetzner. Hard to justify unless "IPFS" is a hard requirement for tooling.

### Option B: Arweave (Permanent Storage)
*   **Model:** Pay Once, Store Forever.
*   **Cost:** ~$3.00 - $6.00 per GB (One-time).
*   **Break-even vs Hetzner:** ~100 Years.
*   **Verdict:** You pay a massive premium for "Immutability" and "Decentralization."

### Option C: Filecoin (Leased Storage)
*   **Model:** Storage Deals (e.g., 6 months). Must be renewed.
*   **Cost:** Extremely low (often near 0 due to subsidies), but requires an "Aggregator" (like Lighthouse or Web3.storage) to manage renewals.
*   **Verdict:** Cheaper than Arweave, but higher maintenance complexity (deal renewal risks).

---

## 3. The Registry Layer (Source of Truth)

### Why Web3? (vs. Web2 Server)
Using a Blockchain Registry instead of a SQL Database changes the trust model:
1.  **Transparency:** Once registered, no one (including the platform owner) can delete the mapping. It solves the "Kill Switch" problem.
2.  **Cryptographic Provenance:** Every entry is signed by the author's private key. Authorship is mathematically verifiable and impossible to spoof.
3.  **Permanence:** The registry runs on thousands of nodes. It does not vanish if the project maintainers stop paying the AWS bill.
4.  **Open Ecosystem:** The registry is permissionless. Third parties can build marketplaces, voting systems, or payment rails on top of these IDs without needing an API key.

### Option A: Base (EVM)
*   **Gas Fee:** ~$0.01 - $0.05 per write.
*   **Pros:** Standard tooling, easy indexing.

### Option B: NEAR
*   **Gas Fee:** < $0.001 per write.
*   **Pros:** Cheaper gas, but requires storage staking for registry state.

---

## 4. Final Recommendation

We have a trade-off between **Ideology (Web3)** and **Economics (Web2)**.

### Path 1: The "Pure Web3" Route (Maximum Censorship Resistance)
*   **Registry:** Base (EVM)
*   **Storage:** Arweave
*   **Cost:** ~$0.02 per publish. $0 monthly.
*   **Why:** True "Proof of Skill." No one can delete the strategy.

### Path 2: The "Pragmatic Hybrid" Route (Best UX/Cost)
*   **Registry:** Base (EVM) (Stores the Hash + Signature)
*   **Storage:** Hetzner / S3 (Stores the Content)
*   **Cost:** ~$0.01 per publish. Negligible monthly.
*   **Risk:** If the server goes down, the content is lost, even if the Registry points to it.
*   **Mitigation:** The Registry stores the **Content Hash (SHA256)**. Anyone can mirror the file. If the official Hetzner server dies, the community can re-upload the file to IPFS/Arweave and the Registry Hash still validates it.

**Decision:**
We will proceed with **Path 1 (Arweave)** for the MVP to prove the "Immutable" value proposition. If costs scale too high, we can switch the storage backend to Hetzner without changing the Registry contract (which just stores a URL/Hash).
