# Evaluation Experiment Plan

This note is the execution plan for the ICSE evaluation section. The current
paper design uses three RQs:

- RQ1: Can PyCGuard find real bugs in the wild?
- RQ2: How does PyCGuard compare with existing Python native-extension bug
  detectors, and what bugs do prior tools find that PyCGuard misses?
- RQ3: How do pruning and structured LLM checking affect cost, robustness, and
  detection quality?

## Existing Evidence

### RQ1: In-the-wild bug finding

Status: already measured and manually audited.

Current source of truth:

- `evaluation/results/tested_104_bugs.json`
- `evaluation/results/manual_review_fp.json`
- `baselines/results/baseline_summary.md`

Paper table:

| Metric | Value |
|---|---:|
| Projects | 10 |
| High-confidence reports | 104 |
| True positives | 86 |
| False positives | 18 |
| Precision | 82.7% |

Remaining work:

- Keep the table in the paper synchronized with `baseline_summary.md`.
- If more issue confirmations arrive, update only the confirmation/fix count;
  do not change the audited precision table unless the 104-report audit changes.

### RQ2: Comparison with prior tools

Status: PyRefcon is the only quantitative baseline. CpyChecker, Pungi, RID, and
CHARON are discussed as unavailable or non-direct baselines.

Current source of truth:

- `baselines/results/baseline_summary.md`
- `baselines/results/pyrefcon_audit_results.jsonl`
- `baselines/results/pyrefcon_audit_units.jsonl`
- `baselines/results/cpychecker_full.jsonl`

Paper table:

| Tool | Unit | TP | FP | Precision |
|---|---|---:|---:|---:|
| PyRefcon | 80 deduplicated audit units | 6 | 74 | 7.5% |

Required narrative:

- PyRefcon finds 6 real defects; 1 overlaps PyCGuard.
- The other 5 are real C memory bugs but do not expose a Python/C API trigger.
- They are therefore outside obligations derived from CPython documentation and
  Python/C API misuse cases.
- CpyChecker is unavailable in a modern reproducible toolchain; Pungi/RID have
  no public implementation; CHARON is adjacent and has no public artifact.

## RQ3 Experiments To Run

RQ3 should validate internal design choices, not introduce another external
baseline. It has three experiments.

### RQ3.1: Static pattern-based pruning cost and safety

Question:

- Does static pattern-based pruning reduce the number of candidate obligations sent to the
  LLM?
- Does pruning remove any known real bug before LLM checking?

Experimental unit:

- Function-level candidate obligation: `(project, file, function, SO)`.

Inputs:

- Full project target manifest: `baselines/results/targets_full.jsonl`.
- Known bug map: `evaluation/bug_so_mapping.json`.
- Source roots: `/tmp/{project}_pr`.

Metrics:

- Number of analyzed source files.
- Number of candidate functions before and after pruning.
- Number of candidate obligations before and after pruning.
- Obligation reduction percentage.
- Known bugs bound before pruning.
- Known bugs retained after pruning.
- Known bugs pruned before LLM checking.
- Per-project breakdown for the same metrics.

Command:

```bash
python3 evaluation/scripts/run_pruning_ablation.py \
  --targets baselines/results/targets_full.jsonl \
  --bug-map evaluation/bug_so_mapping.json \
  --output evaluation/results/rq3_pruning_ablation.json
```

Quick smoke test:

```bash
python3 evaluation/scripts/run_pruning_ablation.py --limit 5 \
  --output /tmp/rq3_pruning_smoke.json
```

Expected paper claim if results hold:

- Pruning substantially reduces candidate obligations and LLM calls.
- Pruning retains all or nearly all known true bugs. If any true bug is pruned,
  report and explain the exact RI gap.

### RQ3.2/RQ3.3: Prompt structure and cross-model robustness

Question:

- Does the structured discharge prompt improve judgment quality over a direct
  verdict prompt?
- Does the same obligation formulation remain effective across multiple LLMs?

Experimental unit:

- Known reviewed case: one `(project, file, function, SO)` from the 104 TP cases,
  optionally plus the 18 manually audited FP cases.

Prompt variants:

- `structured`: trigger identification, path enumeration, DC checks, final
  verdict.
- `direct`: immediate SAFE/VIOLATED verdict with concise evidence.

Inputs:

- TP cases: `evaluation/bug_so_mapping.json`.
- Optional FP cases: `evaluation/results/manual_review_fp.json`.

Metrics:

- TP: number of known true bugs reported as `VIOLATED`.
- FP: number of reviewed false positives still reported as `VIOLATED`.
- Overall correct judgments on reviewed cases, used as a diagnostic metric.
- Malformed JSON rate.
- Unknown/error verdict rate.
- Average latency and token usage if the API returns usage metadata.
- Representative failure categories.

Model set:

RQ1 was run with `claude-sonnet-4-6`; the paper should disclose this in the
experimental setup. For RQ3, use a separate cross-family robustness set so that
the result is not tied only to the RQ1 backend.

The primary RQ3 set should cover GPT, Gemini, DeepSeek, and Qwen while avoiding
an unfair mix of explicit reasoning models and ordinary chat/code models:

| Role | Preferred model | Fallback if unavailable | Rationale |
|---|---|---|---|
| GPT family | `gpt-4o` | `gpt-4o-mini` for budget-only runs | JitVul and ReasonVul both use GPT-4o-class models for vulnerability detection. |
| Gemini family | `gemini-2.5-flash` | `gemini-2.5-pro` if Flash is unavailable or stronger setting is needed | ReasonVul uses Gemini 2.5 Flash; FPA also evaluates Gemini-family code analysis. |
| DeepSeek family | `deepseek-v3-0324` | `deepseek-v3` or `deepseek-coder-33b` | ReasonVul uses DeepSeek-V3; this avoids mixing the explicit reasoning model DeepSeek-R1 into a non-reasoning comparison. |
| Qwen family | `qwen2.5-coder-32b-instruct` | `qwen-max` | VulnAgent-R2 uses Qwen2.5-Coder-32B-Instruct for repository-level vulnerability auditing; ReasonVul uses qwen-max. |

Recent model-selection evidence:

| Work | Venue/status | Models used | Implication for PyCGuard |
|---|---|---|---|
| JitVul, practical repository-level vulnerability detection | arXiv 2025 | GPT-4o-mini and GPT-4o, with plain LLM, dependency-augmented LLM, ReAct, CoT, and few-shot variants | Directly supports reporting prompt-structure effects for vulnerability detection. |
| ReasonVul, multi-perspective vulnerability detection | arXiv 2026 | gpt-4o-2024-05-13, qwen-max, gemini-2.5-flash, deepseek-v3-0324, plus local Llama/Phi/Devstral models | Best direct evidence for the GPT/Gemini/DeepSeek/Qwen family set. |
| VulnAgent-R2, repository-level vulnerability auditing | arXiv 2026 | Qwen2.5-Coder-32B-Instruct; baselines also include DeepSeek-Coder-33B and Code Llama-34B | Supports using a code-specialized Qwen model for repository-level audit style tasks. |
| Frontier cybersecurity benchmark | arXiv 2026 | GPT-family, Gemini-family, Claude Sonnet 4.6, and Qwen3-derived domain-specialized models | Useful only as supporting evidence that frontier/security evaluations report exact backend versions and false-positive behavior. |

Source URLs:

- JitVul: https://arxiv.org/abs/2503.03586
- ReasonVul: https://arxiv.org/abs/2605.18153
- VulnAgent-R2: https://arxiv.org/abs/2603.13384
- Frontier cybersecurity benchmark: https://arxiv.org/abs/2605.23243

Command:

```bash
python3 evaluation/scripts/run_llm_ablation.py \
  --prompt-mode structured \
  --prompt-mode direct \
  --model gpt-4o \
  --model gemini-2.5-flash \
  --model deepseek-v3-0324 \
  --model qwen2.5-coder-32b-instruct \
  --include-fps \
  --output evaluation/results/rq3_llm_ablation.jsonl
```

Quick dry run:

```bash
python3 evaluation/scripts/run_llm_ablation.py --limit 2 --dry-run \
  --output /tmp/rq3_llm_dryrun.jsonl
```

Expected paper claim if results hold:

- Structured discharge checking should reduce unsupported VIOLATED judgments and
  malformed outputs compared with direct prompting.
- The paper should avoid claiming that hidden chain-of-thought alone is the
  contribution; the contribution is structured obligation discharge.
- Robust performance across models supports the claim that obligation-guided
  checking makes the task less model-specific and more consistent.
- Do not claim model capability is irrelevant.
- If budget is tight, run the full structured/direct experiment on
  `gpt-4o`, `gemini-2.5-flash`, and `deepseek-v3-0324`; use Qwen as the fourth
  model when available.

Paper table layout:

| Prompt | Metric | gpt-4o | gemini-2.5-flash | deepseek-v3-0324 | qwen2.5-coder-32b-instruct |
|---|---|---:|---:|---:|---:|
| structured | TP | x / 86 | x / 86 | x / 86 | x / 86 |
| structured | FP | x / 18 | x / 18 | x / 18 | x / 18 |
| direct | TP | x / 86 | x / 86 | x / 86 | x / 86 |
| direct | FP | x / 18 | x / 18 | x / 18 | x / 18 |

Interpretation:

- Higher TP is better.
- Lower FP is better.
- A separate diagnostic paragraph can report malformed JSON, unknown verdicts,
  latency, and notable failure categories.

## Result Summarization

After running RQ3 scripts, generate a Markdown summary:

```bash
python3 evaluation/scripts/summarize_evaluation_experiments.py \
  --pruning evaluation/results/rq3_pruning_ablation.json \
  --llm evaluation/results/rq3_llm_ablation.jsonl \
  --output evaluation/results/evaluation_experiment_summary.md
```

The generated LLM table uses models as columns and prompt/metric pairs as rows,
so RQ3.2 and RQ3.3 can be reported in one compact paper table.

## Proposed Evaluation Section Structure

1. Experimental setup
   - Projects and source versions.
   - PyCGuard configuration.
   - LLM model, temperature, output schema.
   - Manual audit protocol.

2. RQ1: In-the-wild detection
   - Project-level TP/FP table.
   - FP taxonomy.
   - Maintainer confirmation status.

3. RQ2: Comparison with prior tools
   - PyRefcon quantitative comparison.
   - CpyChecker/Pungi/RID/CHARON availability and scope explanation.
   - Analysis of PyRefcon-only TPs.

4. RQ3: Design analysis
   - Pruning cost reduction and bug retention.
   - Structured prompt versus direct prompt.
   - Cross-model robustness.

5. Threats to validity
   - Manual audit bias.
   - Source snapshot drift.
   - LLM nondeterminism and API changes.
   - Baseline reproducibility limits.
