# Skill: Performance Kernel Analysis

## Mission
Given a source directory or file, read the code end-to-end and produce a performance analysis report covering:
- What was optimized and where the hot paths are.
- How each optimization works at the hardware level.
- What trade-offs were accepted (maintainability, portability, numerical precision).
- What to optimize next, ranked by impact and effort.

## Input
- One or more source file or directory paths.
- (Optional) Target ISA, workload shape, or known bottleneck hints.

## Output
- A structured markdown report following the template below.

## When to Use
- Reverse-engineering performance-critical kernels before refactoring or porting.
- Reviewing low-level optimizations in code review.
- Building architecture-aware documentation for onboarding.
- Prioritizing an optimization roadmap for a kernel or runtime path.

## Analysis Dimensions
Cover every applicable dimension. Skip dimensions that genuinely do not apply; never pad.

| Dimension | What to look for |
|-----------|-----------------|
| C++ mechanics | Templates, constexpr, inlining hints, aliasing (`__restrict__`), object lifetime, branch shaping (`[[likely]]`) |
| ASM / ISA | Intrinsics, SIMD packing, instruction selection, predication/masking, software pipelining |
| Microarchitecture | Dependency chains, port pressure, throughput vs. latency, unroll depth, register pressure |
| Memory hierarchy | Blocking/tiling, layout transforms, alignment, prefetch, streaming stores, working-set sizing |
| CPU topology | Vector width selection, front-end decode limits, NUMA placement, cache-line sharing |
| Algorithm | Decomposition strategy, complexity class, data-movement minimization, fusion/fission |
| Parallelism | Thread partitioning, reduction strategy, synchronization cost, false-sharing avoidance |
| Compiler interaction | Optimization barriers, UB reliance, pragmas/attributes, autovectorization friendliness |

## Workflow

### 1. Scope
- Identify target files, hot paths, and intended workload.
- State assumptions explicitly when runtime profiling data is unavailable.

### 2. Design Reconstruction
- Map module boundaries, control flow, data flow, and key invariants.
- Note dispatch logic (JIT selection, runtime feature detection).

### 3. Hot Path Identification
- Mark kernel loops and functions; explain *why* they are hot (call frequency, data volume, algorithmic centrality).

### 4. Optimization Extraction
- Enumerate every optimization mechanism in hot paths.
- Classify each by dimension (table above).

### 5. Hardware Mapping
- For each mechanism, explain the specific CPU behavior it exploits (e.g., "uses FMA port 0/1 dual-issue on Zen 4" or "fits L1 tile to avoid L2 spill").

### 6. Trade-off Evaluation
- Assess: complexity cost, maintainability burden, portability limits, numerical stability impact, corner-case correctness risk.

### 7. Validation Plan
- Specify how to confirm each finding: benchmark scenario, hardware counters, expected metric deltas.

### 8. Recommendations
- Propose next optimizations ranked by Impact / Effort / Risk (see scoring below).

## Evidence Rules
- Every major claim must cite code evidence: file path, function name, line number when possible.
- Label conclusions: **Confirmed** (code proves it), **Strong Inference** (evidence strongly implies it), **Hypothesis** (plausible but unverified).
- Separate observed facts from assumptions. Never say "this is faster" without stating the mechanism and evidence.

## Report Template

### 1. Executive Summary
Core design idea, top 3 performance levers, primary bottleneck(s). Keep to one paragraph.

### 2. Architecture and Dataflow
Components, call graph, data movement path. Include a diagram (ASCII or Mermaid) if the flow is non-trivial.

### 3. Hot Path Deep Dive
Per critical loop/function: compute vs. memory behavior, iteration count model, dominant cost.

### 4. Optimization Catalog
| # | Optimization | Location | Dimension | Mechanism | Expected Benefit | Confidence |
|---|---|---|---|---|---|---|

### 5. Hardware / ISA Interpretation
Map optimizations to vector units, execution ports, memory hierarchy levels, and likely limiting resource.

### 6. Memory and Cache Analysis
Working-set shape, reuse distance, cache-fit assumptions, bandwidth vs. compute balance.

### 7. Algorithmic Analysis
Complexity class, decomposition strategy, movement-to-compute ratio.

### 8. Concurrency Analysis
Threading model, partition granularity, synchronization cost, contention and false-sharing risks.

### 9. Risks and Technical Debt
Portability limits, readability cost, brittle assumptions, correctness gaps, compiler-version sensitivity.

### 10. Recommendations
| # | Change | Impact | Effort | Risk | Dependencies | Validation |
|---|---|---|---|---|---|---|

Priority rule: rank High-Impact + Low-Effort + Low-Risk first.

### 11. Evidence Appendix
Consolidated code references and critical snippets supporting key claims.

## Recommendation Scoring
Each recommendation carries a triple:
- **Impact**: High / Medium / Low — expected speedup or resource saving.
- **Effort**: High / Medium / Low — implementation and testing cost.
- **Risk**: High / Medium / Low — regression, correctness, or portability danger.

## Acceptance Criteria
A report is complete when it:
1. Addresses every applicable analysis dimension (skipped dimensions noted with rationale).
2. Backs every major claim with code evidence and a confidence label.
3. Explains optimization *mechanisms*, not just symptoms or names.
4. Provides at least 3 prioritized, actionable recommendations with validation plans.
5. Is self-contained — a reader unfamiliar with the code can follow the argument.

## Example
- **Input**: `src/cpu/x64/jit_uni_x8s8s32x_conv_kernel.cpp`
- **Output**: Full report covering JIT code-generation strategy, int8 quantization packing, VNNI exploitation, tiling for L1/L2, thread decomposition across spatial dims, and a roadmap for AMX migration.
