# PyCGuard Paper Fact Sheet

This file controls factual consistency across the manuscript. The paper must
not introduce a number, scope statement, or implementation claim that
conflicts with this sheet. Items under `Submission blockers` are deliberately
not treated as established facts.

## Central Claim

PyCGuard represents Python/C API safety rules as safety obligations with
explicit discharge conditions. It uses static analysis to bind and prune
obligation instances and an LLM to check whether the relevant paths discharge
each retained obligation.

## Terminology

| Term | Stable meaning |
|---|---|
| Safety obligation (SO) | A safety property induced by Python/C API use. |
| Discharge condition (DC) | A sufficient code-level condition for satisfying an obligation on a relevant path. |
| Risk indicator (RI) | A static-analyzer pruning pattern used only to decide whether an obligation instance should reach LLM checking. It is not part of the SO/DC specification or evidence that a violation exists. |
| Candidate | One native function containing one or more API calls that bind to at least one SO. |
| Report | One retained function and SO pair judged `VIOLATED` with high confidence. |

Use `PyCGuard` for the tool. Use `Python native extension`, not `Python
program`, for the analysis target. Use `Python/C API` for the interface.

## Construction Artifacts

Authoritative source: `obligations/obligations.json`.

| Artifact | Count | Evidence |
|---|---:|---|
| Safety obligations | 13 | `obligations.json` entries SO-01 to SO-13 |
| Discharge conditions | 41 | All `discharge_conditions` entries |
| Indexed trigger API names | 147 | `tools.static_analyzer.api_index.build_api_index()` |

Static-analysis artifact: `tools/static_analyzer/pruning.py` owns 15 pruning
patterns grouped under eight SO categories. These patterns are deliberately
not stored in `obligations.json`.

The obligation families are reference ownership, reference validity,
null/error validation, error propagation and cleanup, exception state,
resource protocols, post-release safety, type/signature agreement, numeric
and bounds safety, concurrency, garbage-collection lifecycle, container
iteration, and capsule-name lifetime.

The current evidence supports the statement that obligations were derived
from CPython documentation and real bug cases. It does not yet support a
specific count of documents, bug cases, annotators, or agreement rate.

## Detection Pipeline

1. `project_scanner.py` discovers C/C++ source and headers through a direct
   `Python.h` include or Python/C API symbols.
2. Tree-sitter parses C and C++ and extracts call sites.
3. `api_index.py` maps API names to SOs; `matcher.py` groups trigger sites by
   enclosing function and extracts trigger variables when possible.
4. `slicer.py` records trigger-variable references used by static pruning. The
   current LLM input is the full enclosing function, not a reduced slice.
5. `pruning.py` implements 15 pattern checks for SO-01, SO-02, SO-03, SO-05,
   SO-06, SO-07, SO-08, and SO-09. Obligations without an implemented checker are
   retained. If every SO in a candidate is removed, the current implementation
   falls back to the full SO set for that candidate.
6. `discharge_checker.py` sends the full function, SO, and DC list to
   `claude-sonnet-4-6` with temperature 0. The requested JSON records triggers,
   paths, DC judgments, verdict, evidence, and confidence.
7. Checking stops after the first `VIOLATED` SO in a candidate by default.
   Report generation retains high-confidence `VIOLATED` results.

Do not claim interprocedural analysis, macro expansion, compiler-accurate path
feasibility, formal soundness, or completeness.

## Research Questions

- RQ1: How effectively does PyCGuard find previously unknown bugs in real
  Python native extensions?
- RQ2: How does PyCGuard compare with existing Python native-extension bug
  detectors, and why does it miss bugs found by another detector?
- RQ3: How do static pattern-based pruning and structured discharge checking affect cost,
  detection quality, and robustness across LLM backends?

## Established Evaluation Results

### RQ1

Authoritative summary: `baselines/results/baseline_summary.md`.

| Metric | Value |
|---|---:|
| Manually reviewed high-confidence reports | 104 |
| True positives | 86 |
| False positives | 18 |
| Precision | 82.7% |
| Findings confirmed by project maintainers | 45 |

The project breakdown in the summary contains nine projects: SciPy, pandas,
NumPy, Pillow, PyTorch, PycURL, ujson, TensorFlow, and psutil. It sums to 104
reports, 86 true positives, and 18 false positives.

`evaluation/results/tested_104_bugs.json` contains 86 `VIOLATED` and 18 `SAFE`
records. `evaluation/results/manual_review_fp.json` separately describes 18
high-confidence `VIOLATED` reports that humans labeled false positives. These
files encode different stages or reruns and must not be presented as a single
raw detector-output file until their provenance is clarified.

The 18 false positives are categorized as borrowed-versus-new reference
semantics (6), complex control flow (3), macro expansion (2), RAII or smart
pointer cleanup (2), interprocedural cleanup (2), state machine reasoning (1),
error propagation (1), and infeasible path reasoning (1).

### RQ2

Authoritative sources: `baseline_summary.md`,
`pyrefcon_audit_units.jsonl`, and `pyrefcon_audit_results.jsonl`.

| Metric | Value |
|---|---:|
| Raw PyRefcon warnings | 224 |
| Deduplicated audit units | 80 |
| True positives | 6 |
| False positives | 74 |
| Precision | 7.5% |
| True positives also found by PyCGuard | 1 |
| PyRefcon-only true positives | 5 |

The five PyRefcon-only findings concern raw-pointer null dereferences or
generic `malloc` and `free` flows without a Python/C API trigger. They are real
C defects but do not instantiate an obligation derived from a Python/C API
contract. This is the scope explanation, not a claim that the bugs are
unimportant.

CpyChecker produced no usable warning under the attempted toolchains. Pungi
has no public implementation available for reproduction. RID is a generic
reference-count inconsistency detector evaluated on the Linux kernel, not a
Python/C-specific detector. CHARON analyzes cross-language vulnerability
flows and had no public artifact located during the survey. These tools do not
receive fabricated quantitative rows.

### RQ3

No final RQ3 measurements are currently available. Keep the following cells
empty in manuscript tables:

- candidate obligations before and after pruning;
- LLM query, token, time, and cost reduction;
- known-bug retention after pruning;
- structured versus direct prompt TP and FP counts;
- GPT, Gemini, DeepSeek, and Qwen results;
- malformed-output rate, agreement, latency, and token use.

The planned model set and commands are in
`notes/evaluation_experiment_plan.md`. RQ1 used `claude-sonnet-4-6`; this model
choice must be disclosed even if RQ3 uses other backends.

## Related-Work Positioning

- PyRefcon and Pungi are direct Python/C reference-count analyses. PyCGuard
  covers a broader set of Python/C API obligations.
- Multilanguage analysis by Monat et al. analyzes Python and C together for
  language errors. PyCGuard instead checks API-induced native-code duties.
- CHARON follows vulnerability data flows across scripting and native code.
  PyCGuard analyzes the native side and checks Python/C API contracts.
- Safe4U is the closest conceptual LLM work because it decomposes unsafe Rust
  contracts before checking wrappers. The domain, checked unit, and pruning
  strategy differ.
- RFCAudit checks protocol implementations against RFC specifications.
  PyCGuard converts API documentation and bug evidence into reusable safety
  obligations rather than checking a complete external protocol document.
- LLM-integrated static analysis by Li et al. filters static warnings with an
  LLM. PyCGuard uses static analysis to formulate a constrained discharge
  problem before LLM checking.

## Submission Blockers

1. Maintainer status: 45 findings were confirmed by project maintainers. The
   issue-level evidence table should remain part of the artifact record.
2. Construction protocol: record the CPython documentation version, selected
   chapters, bug-corpus source and size, coding procedure, and review process.
3. Subject versions: record one commit hash or release for every evaluated
   project.
4. RQ3: run and audit all planned experiments. Empty cells cannot remain in the
   submitted paper.
5. LLM setup: record provider, exact model identifier, access date,
   temperature, maximum output tokens, retry policy, and total cost.
6. Artifact: prepare an anonymous artifact and add its availability statement.
