# ICSE 2027 Writing Guidelines for PyCGuard

This document is the style and evidence contract for the paper. It combines
the official ICSE 2027 Research Track rules with the useful parts of
`Leey21/awesome-ai-research-writing` and the `ml-paper-writing` and
`systems-paper-writing` skills linked from that repository.

## 1. Submission Constraints

- Use `\documentclass[10pt,conference]{IEEEtran}` without `compsoc` or
  `compsocconf`.
- The main text may occupy at most 10 pages, including figures, tables, and
  appendices. References may occupy at most two additional pages.
- Preserve the official spacing, fonts, and margins.
- The review is double anonymous. Omit author names, refer to prior author work
  in the third person, and use an anonymous artifact link.
- The paper must explain artifact availability or justify why the artifact
  cannot be shared.
- Every citation must be accurate and traceable. ICSE 2027 states that a
  confirmed fabricated or unverifiable reference causes desk rejection.
- Generative artificial intelligence used to create paper content must be
  disclosed according to the IEEE and ACM policies quoted by the call for
  papers. Editing equivalent to grammar correction does not require the same
  disclosure under the ACM wording.

Official source:
https://conf.researchr.org/track/icse-2027/icse-2027-research-track

## 2. Global Narrative

The paper has one thesis:

> Python/C API safety rules become checkable when they are represented as
> safety obligations with explicit discharge conditions; static analysis can
> bind and prune obligation instances, while constrained LLM reasoning checks
> whether each relevant path discharges the obligation.

Every section serves this thesis:

- Introduction: establish the API-contract problem, the limitation of narrow
  reference-count checkers and unconstrained LLM bug finding, and the paper
  contributions.
- Background and Motivation: define the native-extension execution model,
  explain why local API calls induce path-sensitive duties, and show one bug.
- Safety Obligation Construction: explain the evidence sources, abstraction
  process, and the semantic SO/DC representation.
- Detection Pipeline: explain candidate generation, obligation binding,
  static pattern-based pruning, structured checking, and report generation.
- Implementation: report concrete implementation choices without repeating
  the design rationale.
- Evaluation: answer the three RQs using only measured data and describe the
  unit of analysis for every table.
- Discussion and Threats: state scope boundaries, false-positive causes,
  LLM limitations, and validity threats without weakening the central claim.
- Related Work: compare by technical dimension, not paper by paper.
- Conclusion: restate the problem, method, and supported result only.

## 3. Page Budget

| Part | Target pages |
|---|---:|
| Abstract and Introduction | 1.3 |
| Background and Motivation | 0.8 |
| Safety Obligation Construction | 1.5 |
| Detection Pipeline and Implementation | 2.1 |
| Evaluation | 2.8 |
| Discussion and Threats | 0.7 |
| Related Work | 0.7 |
| Conclusion | 0.1 |
| Total main text | 10.0 |

The introduction should end on page 2 or earlier. The first overview figure
should appear within the first three pages. Evaluation receives the largest
budget because ICSE explicitly evaluates rigor, verifiability, and
transparency.

## 4. Paragraph and Sentence Rules

- Give each paragraph one function. Use the first sentence for its claim and
  the remaining sentences for evidence, mechanism, or consequence.
- Put existing context before new information. Keep subjects close to verbs.
- Prefer concrete verbs: `analyze`, `bind`, `prune`, `check`, and `report`.
- Use plain technical language. Avoid promotional words such as `novel`,
  `powerful`, `remarkable`, `showcase`, `leverage`, and `unveil` unless the
  sentence contains evidence that requires the word.
- Avoid mechanical transitions, generic importance claims, em dashes, and
  noun possessives. Do not use contractions.
- Do not use bold or italics as a substitute for logical emphasis in prose.
  Italics remain acceptable for first definitions and RQ labels.
- Use the same term for the same concept throughout. Do not alternate between
  `rule`, `contract`, `property`, and `obligation` after the formal term has
  been introduced.
- Captions must be self-contained and state what is measured or represented.
  Avoid openings such as `This figure shows`.

## 5. Evidence and Citation Rules

- A numerical claim must point to a repository artifact in
  `paper_fact_sheet.md` before it enters the paper.
- Separate detector output, manual ground truth, maintainer feedback, and
  baseline output. Never use one as evidence for another.
- State the unit of analysis explicitly, such as report, function-obligation
  pair, deduplicated warning, source file, or project.
- Report filtering before precision. The current RQ1 precision applies only to
  manually reviewed high-confidence reports, not all detector outputs.
- Do not call a report a confirmed bug unless a maintainer response, merged
  fix, or independent reproducer supports that status.
- Retrieve bibliographic metadata programmatically from DOI, DBLP, Crossref,
  an official proceedings page, or an official project document. Verify that
  the cited paper supports the adjacent claim.
- If a reference or claim cannot be verified, omit it or mark it as a visible
  TODO in the source. Never create plausible metadata from memory.
- Distinguish observation from inference. Use `suggests` when the evidence does
  not establish causality.

## 6. Figures and Tables

- Use Figure 1 to communicate the full information flow: documentation and
  bug cases produce obligations and discharge conditions; source code produces
  candidates; static patterns prune candidates; structured checking produces
  reports.
- Keep generated artwork non-evidentiary. Add critical labels, arrows, and all
  numerical data in LaTeX or another deterministic editing step when needed.
- Use `booktabs` for result tables. The grouped SO/DC specification table may
  use a full grid to make merged cells and condition membership explicit.
- Leave unavailable RQ3 cells empty. Do not insert estimated values, `TBD`
  values that resemble data, or expected improvements.
- Explain every figure or table in the body and state the main observation.
- Use color only when it remains distinguishable in grayscale and for common
  forms of color-vision deficiency.

## 7. Claim Discipline

Permitted claims must match the evidence:

- PyCGuard checks Python/C API safety obligations. It is not a general C/C++
  memory-safety analyzer.
- The LLM performs structured path reasoning over source text. Do not describe
  the result as a sound proof or complete path enumeration.
- Static pattern-based pruning is a heuristic optimization. Its safety must be
  evaluated by known-bug retention.
- Cross-model results, when available, support robustness across the tested
  backends only. They do not show that model capability is irrelevant.
- RQ2 supports a comparison with PyRefcon under the stated reproduction and
  audit protocol. Unavailable tools belong in prose, not in a quantitative
  table with invented zero values.

## 8. Final Review Checklist

- The abstract, introduction contributions, section claims, RQs, and conclusion
  use the same counts and terminology.
- Each contribution maps to at least one evaluation result or a clearly scoped
  construction artifact.
- Every table cell has a source or is empty.
- Every citation is used and verified; every BibTeX entry used by the paper is
  traceable.
- The PDF has at most 10 main-text pages and two reference pages.
- The PDF contains no author identity, broken reference, clipped table,
  unreadable figure, placeholder note, or formatting warning that affects
  readability.
- The acknowledgements or another permitted location contains the required
  generative artificial intelligence disclosure before submission.
