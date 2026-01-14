# AGENT.md

## Purpose

This repository is a **read-only knowledge source** for HubSpot public API specifications.

The AI agent must use this repository **only via the index.json file** to locate relevant specification files and must never infer, guess, or browse paths.

---

## Repository Structure (Authoritative)

- `index.json`
  - The **only authoritative discovery mechanism**
  - Contains all searchable metadata for files in this repo
- `PublicApiSpecs/`
  - Contains the actual JSON specification files
  - Files must only be accessed using paths returned from `index.json`
- `README.md`
  - Human-readable context only
  - Not a discovery source

---

## Index Contract (`index.json`)

The index has the following structure:

- `files[]`
  - `path` (string, required)
    - Exact relative path from repo root to the file
    - This value must be used verbatim when fetching content
  - `keywords` (array of strings)
    - Used for search and filtering only
    - Not guaranteed to be complete or semantic
  - `area` (string)
    - High-level API grouping (e.g. Conversations, Account)
  - `version` (string)
    - API version hint (e.g. v3)

The index **does not contain content**, only file locations and search hints.

---

## Required Agent Behaviour (MANDATORY)

The agent **must follow this sequence exactly**:

1. **Search `index.json`**
   - Match against `keywords`, `area`, and/or `version`
2. **Select one or more entries from `files[]`**
   - Selection must be justified by keyword relevance
3. **Fetch file content**
   - Use the exact `path` value from the selected index entry
4. **Answer using only retrieved file content**
   - Do not rely on prior knowledge or assumptions

---

## Forbidden Actions (STRICT)

The agent **must not**:

- Browse or scan the `PublicApiSpecs/` folder directly
- Infer file paths from folder names
- Guess which file “probably” contains information
- Assume endpoint behaviour not present in the spec
- Merge or reconcile information across files unless explicitly requested
- Treat keywords as authoritative definitions

If no suitable index entry is found, the agent **must stop** and respond:

> “No matching specification found in index.json.”

---

## Scope Clarification

This repository contains:

- OpenAPI / specification data
- Endpoint definitions
- Schema definitions
- Request/response models

This repository does **not** contain:

- Human-written guides
- Conceptual documentation
- Troubleshooting advice
- Business rules beyond what is encoded in specs

---

## Trust Hierarchy

1. File content retrieved via `path`
2. Index metadata (`keywords`, `area`, `version`)
3. Nothing else

If there is a conflict, **file content always wins**.
