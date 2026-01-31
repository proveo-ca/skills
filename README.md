# proveo/skills

**Immutable, Decentralized Context Strategies for AI.**

This project implements a "Proof of Skill" architecture. It moves context management (prompt templates, strategies, heuristics) from local files to a decentralized registry, ensuring transparency, provenance, and censorship resistance.

## Architecture

The system operates in three distinct layers to balance **Trust** (Web3) with **Performance** (Web2).

### Layer 1: The Store (Web3)
*   **Storage:** IPFS (Content-Addressable Storage). Holds the actual strategy files (JSON, Text, Python).
*   **Registry:** EVM Smart Contract (L2). Maps a `Skill_ID` to an `IPFS_CID` and `Author`.
*   **Role:** The immutable "Source of Truth."

### Layer 2: The Indexer (Web2)
*   **Watcher:** A service that listens to blockchain events.
*   **Cache:** A SQL/Vector database that indexes strategies for fast retrieval.
*   **Role:** High-performance caching and searchability.

### Layer 3: The Bridge (Access)
*   **API:** A REST/GraphQL API for AI Agents to query context.
*   **MCP:** A Model Context Protocol adapter for local IDE integration.
*   **Role:** Low-latency delivery to the LLM.

## Usage

1.  **Publish:** Authors upload strategies via CLI.
2.  **Index:** The system automatically detects and indexes the update.
3.  **Consume:** AI Agents query the Bridge API to retrieve the best context for the current task.
