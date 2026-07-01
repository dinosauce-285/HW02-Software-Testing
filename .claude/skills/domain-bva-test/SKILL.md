---
name: domain-bva-test
description: Apply Domain Testing and Boundary Value Analysis to an EShop feature (FR-XX), guiding the AI through every step of the technique and producing a reviewable test-case report plus an AI gap analysis. Use when the user wants to design test cases for a feature, run domain testing / boundary value analysis, or complete Section 6 of HW02.
---

# Domain Testing + Boundary Value Analysis Skill

Covers **Section 6 (Requirements)** of HW02: Domain Testing, Boundary Value
Analysis, and AI gap analysis. Bug reporting is handled by the **Bug Reporter**
procedure in `CLAUDE.md`.

**Core principle (from the assignment):** do NOT issue one generic prompt like
"generate test cases and find bugs". Walk through every step below explicitly and
let the user review each step. Every AI result must be human-reviewed; never
invent inputs, expected values, or execution results — ask the user when unsure.

## Inputs to collect first

- **Feature under test**: `FR-XX` and its name (e.g. `FR-05 Product listing and search`).
- **Specification / business rules**: read the SUT (https://github.com/ttbhanh/eshop-sut)
  or ask the user for the spec, field constraints, and validation rules.
- If constraints are unknown, list your assumptions explicitly and ask the user to confirm.

## Step-by-step procedure

Commit after each step using the Conventional Commits convention in `CLAUDE.md`
(`test(FR-XX): <step>`). Write all output to `reports/test-cases-<FR-XX>.tex`
(LaTeX). The Markdown tables shown below are for readability only — in the report,
render each as a LaTeX `tabular` (use `longtable` for the test-case table so it
breaks across pages), and escape LaTeX specials (`& % $ # _ { }`) in cell text.

### Step 1 — Identify variables
List every input **and** output variable of the feature. For each: name, data
type, and role (input/output). Include implicit variables (e.g. field length,
list size, price, quantity, dates).

| Variable | Type | Input/Output | Notes |
|----------|------|--------------|-------|

### Step 2 — Characterise each variable's domain
For each variable, define its valid domain and all constraints (min, max, format,
required/optional, allowed set). This is the raw material for partitioning.

### Step 3 — Equivalence partitioning (Domain Testing)
For each variable, split the domain into **valid** and **invalid** equivalence
classes. Give every class an ID (`EC-1`, `EC-2`, …) and a one-line rationale.
Cover format classes too (empty, whitespace, wrong type, special chars, injection-like input).

| Class ID | Variable | Valid/Invalid | Description | Representative value |
|----------|----------|---------------|-------------|----------------------|

### Step 4 — Identify boundaries
For every ordered/numeric/length-bounded variable, mark its boundaries. State the
boundary value and whether the boundary itself is inclusive or exclusive.

### Step 5 — Domain matrix (on-point / off-point)
For each boundary build the classic 1×1 domain analysis:
- **on-point**: the value exactly on the boundary.
- **off-point**: the nearest value on the other side of the boundary.
- **in-point / out-point**: a typical interior value and a clearly outside value.
Confirm which side is "pass" — a common bug is an off-by-one at the boundary.

| Boundary | on-point | off-point | Expected on | Expected off |
|----------|----------|-----------|-------------|--------------|

### Step 6 — Boundary Value Analysis test values
Apply **3-value BVA** to each numeric/length boundary: `min-1, min, min+1` and
`max-1, max, max+1` (drop to 2-value only if the user asks). Note the expected
result for each — the invalid side must be rejected with the right message.

### Step 7 — Design the test cases
Combine partitions (Step 3) and boundary values (Steps 5–6) into concrete test
cases. Use single-fault assumption: vary one variable off its valid class while
others stay valid, then add key multi-variable interaction cases.

| TC ID | Technique | Feature | Precondition | Input(s) | Steps | Expected result | Covers (EC/Boundary) |
|-------|-----------|---------|--------------|----------|-------|-----------------|----------------------|

Use `Domain` or `BVA` in the Technique column. Add extra cases where coverage is
thin (the assignment rewards thorough coverage).

### Step 8 — AI gap analysis (mandatory, Section 6.3)
After the AI produces the cases above, critically review them and hunt for what
was likely missed:
- multi-variable interactions and rare combinations,
- **output** boundaries (not just inputs),
- business-rule edge cases (e.g. coupon + price interaction, state transitions),
- locale/encoding, concurrency, and pagination edges.

Add the missing cases, then for each gap record **why** the AI missed it, choosing
from: *prompt quality*, *AI tool limitation*, or *inherent feature complexity*.

| Missed case | Why missed (prompt/tool/complexity) | Added as TC |
|-------------|-------------------------------------|-------------|

### Step 9 — Emit the report
Write everything to `reports/test-cases-<FR-XX>.tex` (LaTeX) with a `\section` for
each step above, so the report shows the full, disciplined derivation (not just the
final table). Use a standalone document (`\documentclass{article}`, `geometry`,
`longtable`, `array`); the test-case table uses `longtable` to page-break.

### Step 10 — Hand off
- Execution: only fill "actual result / pass-fail" after the user runs the tests
  against EShop — never fabricate execution outcomes.
- Any failure → file it with the **Bug Reporter** procedure in `CLAUDE.md`.
- Log this session with the **AI Audit Logger** procedure in `CLAUDE.md`.
