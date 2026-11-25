---
name: design-precedents
description: >-
  Use before proposing or substantially changing a product, UI, API, data model,
  state machine, architecture, or distributed/industrial system design. Study
  mature systems with structurally similar problems to extract transferable
  principles, constraints, failure models, and trade-offs instead of copying
  surface implementations. Do not use for routine bug fixes, small local edits,
  code-only style questions, or post-hoc decoration of an already settled design.
metadata:
  short-description: Design by studying mature precedents first
---

# Design precedents

Study how mature systems solve structurally similar problems **before** choosing a design. The goal is better design judgment, not imitation: prefer a proven boring design unless a concrete constraint justifies novelty.

## Use when

- Designing or substantially changing an object model, state machine, ownership boundary, API/SDK/CLI contract, config format, failure-recovery path, frontend information architecture, or distributed/industrial/realtime workflow.
- Evaluating a new abstraction, synchronization mechanism, or significant user-visible complexity.

## Do not use when

- Fixing routine bugs, making small local changes, or implementing an already approved design.
- Retrofitting examples onto a decision that has already been made.

## Pick depth mode first

| Mode | Use when | Required output |
| --- | --- | --- |
| **Lightweight** | Small or local design choice | 3 lines: **Precedent** / **Lesson** / **For us** |
| **Standard** (default) | Feature, API, UI workflow, or module design | Concise review using the Standard output template below |
| **Deep-dive** | Long-lived contract, cross-service change, state-ownership shift, realtime/hardware/safety constraints, or core abstraction | Read [references/precedent-guide.md](references/precedent-guide.md), inspect primary docs/source across at least two contrasting precedent families, cite sources next to claims, and document rejected precedents |

## Workflow

### 1. Frame the problem structurally

Before searching for precedents, state the real constraints rather than the product category:

- **Users and risk**: who operates the system, how often, and which actions are reversible or dangerous.
- **State and ownership**: primary entities, where authoritative state lives, who mutates it, and what is derived or observed.
- **Execution and failure**: sync vs async, latency/consistency requirements, expected partial failures, and recovery path.
- **Scale and fixed constraints**: current and target scale, hardware or operational limits, and which complexity is essential to the domain versus absorbable by the system.

For industrial, hardware, robotics, calibration, media, or realtime systems, do not default to simple SaaS conventions. Look for structurally matching domains in [references/precedent-guide.md](references/precedent-guide.md).

### 2. Research 2 to 5 strong precedents with verified sources

Match by structural dimensions, not industry label. Check existing dependencies' docs and type definitions first when the repo already depends on a relevant library.

Prefer primary sources in this order:

1. Official architecture/design docs, RFCs, or product reference docs.
2. Source code and repository design docs.
3. Author engineering papers, maintainer talks, or credible technical postmortems.

Avoid basing decisions on SEO tutorials, generic "best practices" lists, context-free screenshots, or unverified forum opinions.

**Evidence rule**: verify how a precedent actually works before citing it. Attach the doc URL, spec, or local file path for verified claims. If you cannot verify a precedent directly, label it `from memory (unverified)` and do not rest a high-stakes choice on it alone.

### 3. Extract the principle and compare constraints

Separate three layers for each precedent:

- **Surface**: what users see (for example, Kubernetes resources written as YAML).
- **Mechanism**: how it works (persistent declarative resource specs).
- **Principle**: why it works (separate desired state from observed state and continuously reconcile).

Transfer the **principle** first, the mechanism when constraints match, and the surface rarely. When two mature systems disagree, trace what differed in their constraints (user model, ownership, consistency, scale, failure model) rather than declaring one universally better.

### 4. Classify what transfers and challenge both extremes

Classify each candidate idea into one bucket:

- **Borrow**: constraints match closely enough to reuse directly.
- **Adapt**: principle transfers, implementation must change for our constraints.
- **Reject**: successful elsewhere, but key constraints differ materially.
- **Investigate**: evidence is insufficient; state what to verify next.
- **New design needed**: existing precedents fail a real constraint (use deliberately, never as default).

Apply two mandatory challenges before recommending a direction:

1. **Challenge novelty**: if proposing a new abstraction, state the concrete constraint that makes existing patterns fail (latency, trust boundary, user model, hardware, consistency, failure model, or a documented limitation). Reject weak excuses ("feels cleaner", "more generic", "modern", "might need it someday").
2. **Challenge historical baggage**: do not copy mechanisms built for 100,000 nodes or a public platform ecosystem into a 10-node or internal system. Maturity is not complexity.

When evaluating evolution and compatibility, separate **active external contracts** (which need explicit evolution rules) from **internal code paths** (where backward compatibility is not a default goal and superseded paths should be removed rather than wrapped in dual paths).

### 5. Settle contested choices with concrete shapes

Do not stop at abstract prose when two precedents conflict or when the classification is `New design needed`:

- Sketch **2 competing shapes** (concrete data structures, state ownership, and interfaces) so the trade-off is visible in real types rather than adjectives.
- If a small prototype or experiment can settle a key uncertainty, prototype and recommend based on observed behavior. Leave pure product or preference choices for the user.

## Output templates

### Lightweight mode

> **Precedent:** how a mature system handles the same structural problem (plus source or `from memory (unverified)`).
> **Lesson:** the underlying principle.
> **For us:** what transfers and what does not.

### Standard and deep-dive mode

Use only the sections that carry real signal:

- **Problem**: structural framing and key constraints.
- **Relevant precedents**: for each, name the system and idea, then state what it does, why, its governing constraint, source (or `from memory (unverified)`), and transferable lesson.
- **Pattern**: shared principle or constraint-driven disagreement across precedents.
- **Application**: concise **Borrow**, **Adapt**, and **Reject** lists.
- **Proposed direction**: recommended design (with concrete types/boundaries when contested), clearly separating borrowed precedent from problem-specific novelty.
- **Open questions**: only questions that materially change the design.

## Anti-patterns

| Anti-pattern | Failure mode |
| --- | --- |
| **Screenshot imitation** | Copying visible UI layout without matching the user model or risk profile |
| **Cargo-cult architecture** | Importing big-company topology (microservices, distributed consensus) without its scale constraint |
| **Framework-driven design** | Letting a library's convenience dictate the domain model |
| **Name-dropping** | Listing famous systems ("Stripe and K8s do X") without explaining the mechanism or constraint |
| **False universality** | Calling a choice "industry standard" without naming the constraints that made it standard |
| **Abstraction worship** | Treating generality as a virtue without a concrete second need |
| **Novelty or tradition bias** | Assuming "we are special" or "existing systems do it this way" without evidence |
