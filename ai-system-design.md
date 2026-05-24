# Design AI Code Review Agent

## System Design — Reviewing Large PRs Beyond Context Limits

---

## Functional Requirements

* Read PRs spanning many files
* Review changed code + nearby context
* Detect cross-file bugs
* Surface risk, comments, and fixes
* Escalate uncertain areas to humans

---

## Non-Functional Requirements

* Full file coverage guarantee
* Low review latency
* High precision on risky changes
* Cost-aware context usage
* Explainable findings + confidence

---

## Scale Constraints

* **180k** PR Tokens
* **128k** Model Context
* **40** Changed Files
* **3,000** Changed LOC

> Most critical bug may hide in file 37 that never fits in a single prompt.

---

# High-Level Architecture

```text
PR / Git Provider
(diff, files, metadata)
        |
        v
+----------------+
|   Diff Parser  |
+----------------+
        |
        v
+----------------------+
| Change Graph Builder |
+----------------------+
        |
        v
+--------------+
| Risk Scorer  |
+--------------+
        |
        +--------------------------+
        |                          |
        v                          v
+----------------+      +------------------+
| Retrieval      |      | External Memory  |
| Engine         |      +------------------+
+----------------+
        |
        v
+----------------------+
| Multi-Pass Reviewers |
+----------------------+
        |
        v
+----------------+
| Final Verifier |
+----------------+
        |
        v
+----------------+
| Review Output  |
+----------------+
```

---

# Inputs from Git Provider

* Diff
* Files
* Metadata

---

# Retrieval Engine Fetches

* Callers
* Tests
* Configs
* Interfaces

---

# External Memory Stores

* File summaries
* Risks
* Dependencies
* Open questions

---

# Multi-Pass Reviewers

## Review Dimensions

* Correctness
* Security
* Performance
* API Compatibility
* Test Coverage

---

# Final Review Output

* Issues
* Confidence
* Files reviewed
* Human escalation

---

# Core Rule

> Scan every file once, deeply review only high-risk areas.

---

# Review Passes

## Pass 1 — Coverage Scan

* Visit every changed file
* Summarize symbols + changes
* Detect initial risk signals

---

## Pass 2 — Deep Review

* Inspect high-risk files
* Fetch surrounding code
* Compare cross-file assumptions

---

## Pass 3 — Verification

* Contradiction checks
* Missing tests / contract drift
* Confidence scoring + escalation

---

# Key Retrieval Questions

1. Which callers use this changed function?
2. Which tests cover this path?
3. Did a schema or API contract change?
4. Which config or feature flag affects runtime?
5. What files still assume old behavior?

---

# External Memory Schema

## FileSummary

| Field      | Description     |
| ---------- | --------------- |
| file_id    | File identifier |
| summary    | File summary    |
| risk_score | Risk score      |

---

## DependencyEdge

| Field    | Description         |
| -------- | ------------------- |
| src      | Source file         |
| dst      | Destination file    |
| relation | Dependency relation |

---

## ReviewState

| Field         | Description           |
| ------------- | --------------------- |
| file_id       | File identifier       |
| scanned       | Coverage scanned      |
| deep_reviewed | Deep review completed |

---

## Issue

| Field      | Description      |
| ---------- | ---------------- |
| file_id    | File identifier  |
| severity   | Issue severity   |
| confidence | Confidence score |

---

# Scaling & Improvements

* Hierarchical summaries
* Symbol-level indexing
* Test impact analysis
* Caching retrieved context
* Human-in-the-loop fallback


<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/12a63a4c-bdff-48af-90c8-2409c1cc9401" />
