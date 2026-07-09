---
name: learn-from-code
description: 'Deep-read project source code to extract and summarize design patterns, design philosophy, architecture decisions, and performance optimization techniques. Use when: learning from unfamiliar codebase; onboarding to a new project; extracting reusable design knowledge; studying how a system solves complex problems; building a knowledge base from production code.'
argument-hint: 'Path to source directory or file(s) to study'
---

# Skill: Learn From Project Code

## Mission
Given source code files or directories, perform a thorough reading and analysis to produce a structured knowledge report covering three pillars:
1. **Design Understanding** — Architecture, module decomposition, data flow, and key invariants.
2. **Design Methods & Patterns** — Extract reusable design patterns, design philosophy, and engineering methodology.
3. **Performance Optimization** — Identify and catalog all performance techniques, tricks, and trade-offs.

## Input
- One or more source files or directory paths.
- (Optional) Focus area: "design only", "performance only", or "full" (default: full).
- (Optional) Domain context or background knowledge hints.

## Output
A structured markdown knowledge report following the template below, suitable for reference and learning.

## When to Use
- Onboarding to an unfamiliar codebase or module.
- Extracting reusable design patterns from production code.
- Studying how experienced engineers solve complex problems.
- Building a personal knowledge base of design methods and performance tricks.
- Preparing for design reviews or architecture discussions.
- Understanding the "why" behind code structure, not just the "what".

## Analysis Dimensions

### Pillar 1: Design Understanding

| Dimension | What to Look For |
|-----------|-----------------|
| Architecture | Module boundaries, layering, dependency direction, plugin/extension points |
| Abstractions | Key interfaces, abstract classes, type hierarchies, concept modeling |
| Data Flow | How data enters, transforms, flows between components, and exits |
| Control Flow | Dispatch logic, state machines, event handling, lifecycle management |
| Invariants | What must always be true; contracts, preconditions, postconditions |
| Error Handling | Error propagation strategy, recovery mechanisms, failure boundaries |
| Configuration | How behavior is parameterized, runtime vs compile-time decisions |

### Pillar 2: Design Methods & Patterns

| Dimension | What to Look For |
|-----------|-----------------|
| Creational Patterns | Factory, Builder, Singleton, Prototype, Object Pool |
| Structural Patterns | Adapter, Bridge, Composite, Decorator, Facade, Proxy |
| Behavioral Patterns | Strategy, Observer, Command, Chain of Responsibility, Visitor, Template Method |
| Architecture Patterns | Plugin architecture, Pipeline, Microkernel, Layered, Event-driven |
| C++ Idioms | RAII, CRTP, Pimpl, Tag dispatch, SFINAE/Concepts, Type erasure, Copy-and-swap |
| API Design | Fluent interfaces, Builder pattern, Named parameters, Strong types |
| Extensibility | Open-closed principle application, extension mechanisms, hook points |
| Separation of Concerns | How responsibilities are divided, coupling control, cohesion |
| Defensive Programming | Assertions, static_assert, compile-time checks, safe defaults |
| Configuration-Driven | Data-driven behavior, table-driven logic, declarative specifications |

### Pillar 3: Performance Optimization

| Dimension | What to Look For |
|-----------|-----------------|
| Algorithm Choice | Complexity class, decomposition strategy, data-movement minimization |
| Memory Management | Allocation strategy, pooling, arena allocation, move semantics, COW |
| Cache Optimization | Blocking/tiling, data layout, alignment, prefetch, working-set sizing |
| SIMD / Vectorization | Intrinsics, auto-vectorization hints, data packing, SOA vs AOS |
| Parallelism | Thread partitioning, lock-free structures, reduction, work stealing |
| Lazy Evaluation | Deferred computation, on-demand initialization, short-circuit evaluation |
| Compile-time Optimization | constexpr, templates for specialization, compile-time dispatch |
| Caching & Memoization | Result caching, LRU, precomputation, lookup tables |
| Branch Optimization | Branch prediction hints, branchless code, likely/unlikely |
| I/O & Serialization | Memory-mapped I/O, zero-copy, batch processing, streaming |
| Precision Trade-offs | Reduced precision (bf16/f16/int8), quantization, approximate computing |

## Workflow

### 1. Scope & Survey
- Identify target files and their role in the overall system.
- Skim directory structure, header files, and README/docs to build a mental map.
- State the domain and purpose of the code being studied.

### 2. Architecture Reconstruction
- Map module boundaries, component relationships, and dependency graph.
- Identify the main entry points and execution flow.
- Draw data flow and control flow (ASCII or Mermaid diagram).
- Identify the key abstractions and their relationships.

### 3. Design Pattern Extraction
- For each identified pattern:
  - **Name** the pattern (use standard names when applicable).
  - **Locate** it: file, class, function.
  - **Explain** how it's implemented in this specific codebase.
  - **Explain why** it was chosen — what problem does it solve here?
  - **Assess** the implementation quality: idiomatic? over-engineered? elegant?
- Group patterns by category (creational, structural, behavioral, architectural).

### 4. Design Philosophy Extraction
- Infer the guiding principles behind the design decisions:
  - What trade-offs were deliberately made (flexibility vs simplicity, performance vs readability)?
  - What conventions are consistently followed?
  - How is complexity managed?
  - What is the error philosophy (fail-fast? defensive? tolerant?)?
- Identify the "design DNA" — the recurring decision patterns that reveal the team's engineering values.

### 5. Performance Technique Extraction
- For each identified optimization:
  - **Name** the technique.
  - **Locate** it: file, function, line range.
  - **Explain** the mechanism: what hardware/runtime behavior is exploited.
  - **Quantify** the expected benefit when possible (O(n) → O(1), cache miss → hit, etc.).
  - **Note** the trade-off: what was sacrificed (readability, portability, generality).
- Classify by dimension (algorithm, memory, parallelism, etc.).

### 6. Cross-Cutting Insights
- Identify design decisions that serve both correctness AND performance.
- Note how design patterns enable or constrain performance optimizations.
- Find examples where the architecture itself is the optimization.

### 7. Lessons Learned & Takeaways
- Distill the most valuable, transferable insights.
- Formulate as actionable principles that can be applied to other projects.
- Rank by universality (specific to this domain vs broadly applicable).

## Evidence Rules
- Every claim must cite code evidence: file path, class/function name, line range when possible.
- Label conclusions: **Confirmed** (code proves it), **Inferred** (evidence strongly implies it), **Hypothesis** (plausible but needs verification).
- Distinguish between "what the code does" and "why it does it that way".
- When the rationale isn't obvious, propose the most likely motivation and label it as inference.

## Report Template

### 1. Executive Summary
One paragraph: what this code does, its core design idea, the 3 most notable design/perf insights.

### 2. Architecture Overview
- Component diagram (Mermaid or ASCII).
- Module responsibilities and dependency direction.
- Key entry points and execution paths.

### 3. Design Patterns Catalog

| # | Pattern | Category | Location | Problem Solved | Implementation Notes | Quality |
|---|---------|----------|----------|---------------|---------------------|---------|

### 4. Design Philosophy
- Guiding principles inferred from the code.
- Trade-off decisions and their rationale.
- Coding conventions and their purpose.
- How complexity is managed.

### 5. Performance Optimization Catalog

| # | Technique | Category | Location | Mechanism | Benefit | Trade-off | Confidence |
|---|-----------|----------|----------|-----------|---------|-----------|------------|

### 6. Performance Deep Dives
For the top 3-5 most significant optimizations, provide detailed analysis:
- What hardware behavior is exploited.
- How the optimization interacts with the surrounding design.
- Conditions under which the optimization is most/least effective.

### 7. Cross-Cutting Insights
- Where design and performance reinforce each other.
- Where they conflict and how the conflict was resolved.

### 8. Transferable Lessons
Ranked list of insights that can be applied to other projects:

| # | Lesson | Domain | Universality | Source Evidence |
|---|--------|--------|-------------|----------------|

### 9. Open Questions
Things that remain unclear and would benefit from further investigation or team discussion.
