# Building the "Context List" Protocol

**Goal:** Immortalize skills for LLMs by creating a neutral, shared infrastructure (The Library of Alexandria for AI).

## 1. The Concept: "Protocol" vs. "Product"
*   **Product (Web2):** A centralized service (SaaS) where we hold the keys. If we disappear, the data disappears.
*   **Protocol (Web3):** A standard set of rules. If followed, the result is guaranteed without asking for permission.
    *   *Analogy:* We are building the "Uniswap Token Lists" for AI Context.

## 2. Core Pillars

### A. The "Shape" of Data (Schemas)
A protocol requires a shared language.
*   **Requirement:** Define a strict JSON Schema for a "Skill".
*   **Example:** Every skill MUST have `title`, `version`, `author`, and `prompt_template`.
*   **Benefit:** Any AI Agent (Claude, GPT, Local Llama) knows exactly how to parse the file, regardless of who uploaded it.

### B. Interfaces (The Handshake)
The Smart Contract is a public promise.
*   **Mechanism:** Solidity Interface / ERC Standard.
*   **The Promise:** "If you give me a `Skill_ID`, I will return the `Arweave_Hash` and the `Author_Address`."
*   **Benefit:** Allows third parties (marketplaces, IDE plugins, voting DAOs) to build on top of the registry without asking `proveo` for an API key.

### C. Immutability & Content Addressing
The shift from "Location" to "Content".
*   **Web2:** `google.com/file.txt` (Location-based. Breaks if moved).
*   **Web3:** `Arweave_Hash_8f9a...` (Content-based. The hash *is* the file).
*   **Benefit:** The "Identity" of the skill is mathematical, not administrative.

## 3. Roadmap

### Phase 1: Define the Schema
*   Create the structure for a Skill.
*   Ensure it covers text prompts, few-shot examples, and metadata.

### Phase 2: Deploy the Registry (Base)
*   Write the Solidity contract.
*   Map `Skill_ID` -> `Arweave_Hash`.
*   Implement "Owner" logic (only the author can update their skill version).

### Phase 3: Store the Data (Arweave)
*   Implement the CLI to upload skill templates to Arweave.
*   Get the Transaction ID (Hash).
*   Send the Hash to the Registry.

### Phase 4: The Bridge
*   Build the API/Indexer that reads the Registry and fetches from Arweave.
*   Serve this to AI Agents via MCP (Model Context Protocol).
