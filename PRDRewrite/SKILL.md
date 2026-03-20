---
name: PRD Rewrite
description: Lossless PRD rewriting with UC 3.0 structure enhancement, section-level splitting, and Indicator/Success Metrics enrichment.
version: 4.0.0
status: active
triggers:
  - "rewrite PRD"
  - "improve PRD"
  - "viết lại PRD"
  - "cải thiện PRD"
  - "upgrade PRD"
---

# PRD Rewrite Skill

Rewrite a PRD to fix issues found during analysis while maintaining **100% of the original content**. Enhances structure with UC 3.0 (Jacobson & Cockburn 2024) and enriches metrics with Indicator/Success frameworks.

## How to Use

When the user asks you to rewrite/improve a PRD:

1. Read the original PRD content
2. Read the analysis results (from PRDAnalysis skill output, if available)
3. For short PRDs (≤3 sections): rewrite the entire document in one pass
4. For large PRDs (>3 sections): split into sections, rewrite each section with targeted issues, reassemble
5. Output the complete rewritten PRD in Markdown

---

## Core Principle: LOSSLESS Editing

> **You are a "LOSSLESS PRD EDITOR"**. The rewritten PRD MUST be **equal length or LONGER** than the original. You must NEVER summarize, truncate, or omit any context, reasoning, or detail from the original PRD. If a section has no issues, **copy it verbatim** into the output.

---

## Mandatory Rules (DO NOT VIOLATE)

1. **DO NOT summarize**, shorten, or skip any context, reasoning, or detail — no matter how small.
2. **PRESERVE ALL identifiers** (UC-xxx, US-xxx, EP-xxx, FR-xxx, NFR-xxx, IDs, codes). Do NOT rename, remove, or renumber them.
3. **PRESERVE ALL tables** — do not convert tables to lists. If adding columns, keep existing rows intact. Keep markdown tables compact (e.g., `|---|---|`).
4. **PRESERVE ALL detailed content** within each item, including sub-bullets, examples, notes, definitions.
5. **ABSOLUTELY NO HTML tags** — use pure Markdown syntax only.
6. Return **ONLY the Markdown content**, no explanations.
7. All content in **Vietnamese**.

---

## Heading Hierarchy Rules

The rewritten PRD MUST follow strict Markdown hierarchy:

| Level | Usage | Example |
|-------|-------|---------|
| `#` | Document title (exactly ONE) | `# PRD: Hệ thống thanh toán V2` |
| `##` | Major sections | `## 1. Executive Summary` |
| `###` | Subsections, Use Cases | `### UC-001: Đặt hàng online` |
| `####` | Structural blocks | `#### User Stories`, `#### Acceptance Criteria` |
| `#####` | Atomic items | `##### US-001.1` |

**Critical rules:**
- Never skip levels (no jump from `##` to `####`)
- Never use bold text instead of headings
- Never use bullet symbols as section headers
- Convert any numbered items used as headings (e.g., `1.`, `2.`, `3.`) into proper `##` headings
- Use Cases MUST be `###`, never `##`
- Structural blocks (Problem, User Stories, AC, Success Metrics, States, Hard Constraints) MUST be `####`

---

## Section Splitting Strategy (for large PRDs)

When the PRD has **more than 3 `##` sections**, split and rewrite each section independently:

1. Split at every `## ` heading (but NOT `###` or deeper)
2. Section 0 = preamble (content before the first `##`, including `#` title)
3. For each section, collect all relevant issues from the analysis results
4. Rewrite each section independently using the section rewrite template
5. Reassemble in original order

---

## Enhancement Requirements

When rewriting, ADD the following if missing (never remove existing content):

### UC 3.0 Structure (Jacobson & Cockburn, 2024)

For each Use Case (UC-xxx) or major business flow in the PRD, add/improve these 9 sections if missing:

1. **UC Header** — Table: UC ID | Title (verb phrase) | Primary Actor | Goal in Context | Scope | Level (Summary ☁️ / User Goal 🌊 / Subfunction 🐟) | Status
2. **Stakeholders & Interests** — Table listing who cares about the outcome and what they expect
3. **Preconditions** — Conditions that MUST be true before the UC starts (e.g., Actor logged in, data available)
4. **Postconditions** — Two parts:
   - *Success Guarantee*: System state when UC succeeds
   - *Minimum Guarantee*: System state even when UC fails (e.g., logs written, no dangling transactions)
5. **Specifications (SPEC-N)** — Table: Spec ID | Description | Priority (Must/Should/Could)
6. **Basic Scenario (Main Success)** — Table: Step | Actor/System | Action. Each step is actor-intention or system-response (3–9 steps, NO IF/ELSE)
7. **Extensions (Alternative & Exception Flows)** — Format: `[Step].[Letter]: [Condition]`. Example: "3a. Out of stock" → steps 3a.1, 3a.2… Include extension `*a` for system errors at any step
8. **Use-Case Slices** — Table: Slice ID (UC-XXX-S1) | Slice Name | Scenario Steps | Priority | Sprint | Story Points | Acceptance Criteria (Given/When/Then)
9. **Special Requirements** — NFRs specific to this UC (Performance, Security, Accessibility)

### Acceptance Criteria

- Add Given/When/Then format for each feature if missing
- Keep existing AC content intact

### Edge Cases & Error Handling

- Add explicit edge cases for each UC if missing
- Document error states, recovery paths, graceful degradation

### Indicator Metrics & Success Metrics

For sections containing metrics/success criteria, add two types:

- **Indicator Metrics (Chỉ số dẫn dắt)**: Measurable input metrics from day one. Examples: API calls/day, form completion rate, avg response time, new feature activation count
- **Success Metrics (Chỉ số thành công)**: Output KPIs tied to business goals. Examples: +20% conversion rate, -15% churn rate, NPS ≥ 50

For each metric, specify: **Name | Formula/Method | Target | Frequency | Data Source**

> Suggest one line about using an AI dashboard to automatically detect anomalies and recommend actions when metrics deviate from targets.

---

## PRD Formatting Rules

When normalizing structure, apply these WBS neutralization rules:

1. **Numbering is label text only** — It does NOT control heading depth. If WBS numbering (1., 1.1, 2.3.4, roman numerals) conflicts with proper Markdown nesting, fix the structure
2. **Semantic grouping over numbering** — Restructure based on meaning, not existing numbering depth
3. **Preserve IDs as text** — UC-001, US-001.1, I-001 are text labels, not heading-level determiners
4. **Tables stay tables** — Never convert tables to lists
5. **No WBS echoing** — Do not replicate `## 6.` / `### 6.1` / `#### 6.1.1` patterns

---

## Section Renumbering

After rewriting, sequentially renumber headings that use numeric prefixes:

- `## <N>. Title` → renumber to 1, 2, 3, ...
- `### <N>.<M>. Title` → renumber to parentH2.1, parentH2.2, ...
- Headings without numeric prefixes (e.g., `## Executive Summary`) are left as-is
- `#`, `####`, `#####` headings are never renumbered
