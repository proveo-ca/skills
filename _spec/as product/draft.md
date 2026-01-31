# Phase 0: The Git-Based MVP ("The Card Catalog")

**Goal:** Immediate utility. Prototype the Schema and Bridge API.

## 1. The Concept
- **GitHub Repository** as the Registry.
- **Git Files** as the Storage.
- **Human Curation** (Pull Requests).

## 2. The Write Path (Publishing & Curation)

**The Workflow:**

1.  **Creation (The "Extraction" Agent):**
    *   **Trigger:** Developer finishes a complex task.
    *   **Action:** Run local script `proveo extract`.
    *   **Logic:** The agent bundles shell history/diffs and asks a local LLM: *"Summarize the strategy used here into a reusable Skill Template."*
    *   **Output:** A file `skills/react/debug-infinite-loop.md` adhering to a strict Frontmatter schema.

2.  **Submission (The PR):**
    *   The script automatically opens a Pull Request: *"feat: Add React Infinite Loop Debugging Strategy"*.

3.  **Validation (The "Contract"):**
    *   **Mechanism:** GitHub Actions (CI).
    *   **Checks:**
        *   Does the file match the JSON/Markdown Schema?
        *   Are required fields (Author, Version, Context) present?
    *   **Result:** Block merge if validation fails.

4.  **Curation (The Human):**
    *   Maintainers review the PR for quality, safety, and redundancy.
    *   **Merge:** The skill is now "Live."

## 3. The Read Path (Consumption)

**The Workflow:**

1.  **The Bridge API:**
    *   A simple Node.js/Python server.
    *   **Startup:** Clones the repo (or fetches tree via GitHub API).
    *   **Indexing:** Builds an in-memory vector index of `description` fields.

2.  **The Agent Request:**
    *   AI Agent (Cursor/Script) calls: `GET https://api.proveo.dev/search?q=react+loop`.

3.  **The Response:**
    *   API returns the raw text of the best-matching skill.

## 4. Agent Integration (System Prompt)

Agents consume the bridge via **Tool Definitions** (Function Calling).

**Tool Spec:**
```json
{
  "name": "get_expert_context",
  "description": "Call this when you are stuck or need a specific strategy for a complex task. Do not guess; look up the best practice.",
  "parameters": {
    "type": "object",
    "properties": {
      "query": {
        "type": "string",
        "description": "The specific topic or error (e.g., 'python memory leak')"
      }
    }
  }
}
```

**Flow:**
1.  **User:** "My React app is freezing."
2.  **Agent:** Calls `get_expert_context("react freezing")`.
3.  **Bridge:** Returns content of `skills/react/debug-performance.md`.
4.  **Agent:** "According to the Proveo registry, we should check for unkeyed lists..."

## 5. Comparison: MVP vs. Protocol

| Feature | Web3 Future (Protocol) | Git MVP (Product) |
| :--- | :--- | :--- |
| **Storage** | Arweave | GitHub Repo (`/skills` folder) |
| **Registry** | Smart Contract | `skills.json` or File Tree |
| **Validation** | Contract Logic | GitHub Actions (CI) |
| **Curation** | Immutable/DAO | Human PR Review |
| **Access** | Bridge API | Bridge API (wraps GitHub) |
