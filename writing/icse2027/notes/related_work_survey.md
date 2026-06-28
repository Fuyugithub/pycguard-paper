# Related Work Survey

This note organizes related work into two groups for the ICSE 2027 paper:

1. Native extension and boundary analysis.
2. LLM-based bug detection.

The collection strategy is to start from recent CCF-A or equivalent top-tier seed papers, then include foundational work cited by those seeds when it directly explains the research lineage.

## Group 1: Native Extension and Boundary Analysis

This group covers Python/C extensions, cross-language native interfaces, and unsafe/native boundary analyses. It should be the first Related Work subsection because PyCGuard directly targets Python native extensions.

### Recent CCF-A or Top-Tier Seeds

#### PyRefcon

- Citation key: `pyrefcon2023`
- Venue: ASE 2023
- Local PDF: `papers/pyrefcon_ase2023.pdf`
- Role: Closest Python/C native extension detector.
- Summary: Tracks PyObject lifecycle states and reference counts with static symbolic execution.
- Difference from PyCGuard: PyRefcon targets reference-count memory errors. PyCGuard generalizes Python/C API misuse into 13 safety-obligation categories and uses LLM reasoning to check discharge conditions on execution paths.
- Baseline status: Primary quantitative baseline for RQ2. PyRefcon is the closest reproducible Python/C native-code analyzer. Its own paper states that Pungi and RID are not publicly available, and therefore compares them only through aggregate numbers from their original papers.

#### CHARON

- Citation key: `charon2025`
- Venue: EuroS&P 2025
- PDF status: Official paper page found; public implementation not found.
- Role: Recent adjacent work on scripting-language native-extension vulnerability detection.
- Summary: Builds a polyglot static analysis over scripting-language and C/C++ code property graphs to detect cross-language vulnerability flows in native extensions.
- Relation to PyCGuard: CHARON is relevant because it targets native extensions in ecosystems such as Python and JavaScript. It is not a direct RQ2 baseline because it targets cross-language vulnerability flows rather than Python/C API safety-obligation violations, and no public tool or artifact was found during our survey.

#### Is unsafe an Achilles' Heel?

- Citation key: `unsafeachilles2024`
- Venue: ICSE 2024
- PDF status: Metadata-only in current environment.
- Role: Recent CCF-A seed for unsafe/native safety requirements.
- Summary: Empirically studies safety requirements in unsafe Rust programming.
- Relation to PyCGuard: Useful for positioning safety requirements as explicit obligations at low-level language boundaries. PyCGuard specializes this idea to Python/C API requirements.

#### Understanding and Detecting Real-World Safety Issues in Rust

- Citation key: `rustsafetytse2024`
- Venue: IEEE TSE 2024
- PDF status: Metadata-only in current environment.
- Role: Recent top-tier journal work on detecting safety issues in Rust.
- Relation to PyCGuard: Reinforces that safety specifications around unsafe/native features can be studied, categorized, and detected with static analysis.

#### Don't Panic!

- Citation key: `dontpanic2025`
- Venue: CCS 2025
- PDF status: Metadata-only in current environment.
- Role: Latest CCF-A security seed for Rust bugs hidden behind runtime safety checks.
- Relation to PyCGuard: Shows that bugs can remain even in ecosystems with strong safety mechanisms, motivating domain-specific safety checking around unsafe/native interfaces.

#### SafeDrop

- Citation key: `safedrop2023`
- Venue: TOSEM 2023
- PDF status: Metadata-only in current environment.
- Role: Static data-flow analysis for Rust memory deallocation bugs.
- Relation to PyCGuard: Similar in modeling resource/lifetime misuse, but PyCGuard targets Python/C API obligations and combines static pruning with LLM path reasoning.

#### MirChecker

- Citation key: `mirchecker2021`
- Venue: CCS 2021
- PDF status: Metadata-only in current environment.
- Role: Static analysis for Rust bugs.
- Relation to PyCGuard: Useful contrast for compiler-intermediate-representation based bug detection. PyCGuard instead works on C/C++ extension source and Python/C API semantics.

#### Understanding Memory and Thread Safety Practices in Rust

- Citation key: `rustsafetypldi2020`
- Venue: PLDI 2020
- PDF status: Metadata-only in current environment.
- Role: CCF-A seed on real-world unsafe Rust practices and memory/thread safety.
- Relation to PyCGuard: Supports the broader observation that practical systems mix high-level safety with low-level escape hatches that require specialized analysis.

#### How Do Programmers Use Unsafe Rust?

- Citation key: `unsaferustoopsla2020`
- Venue: OOPSLA 2020
- PDF status: Metadata-only in current environment.
- Role: Empirical foundation for unsafe code usage.
- Relation to PyCGuard: Useful motivation that unsafe/native boundaries are common and varied.

### Python/C-Specific and Boundary Foundations from Citation Chains

#### Pungi

- Citation key: `pungi2014`
- Venue: ECOOP 2014
- PDF status: Metadata-only in current environment.
- Role: Early Python/C reference-counting analysis.
- Summary: Uses affine analysis to find Python/C reference-counting errors.
- Relation to PyCGuard: Foundational related tool. PyCGuard is broader than reference counting and avoids reducing all reasoning to affine reference-count equations.
- Baseline status: Not included in quantitative RQ2 because the tool is not publicly available; PyRefcon made the same choice and only compared with Pungi through aggregate numbers reported in the original paper.

#### RID

- Citation key: `rid2016`
- Venue: ASPLOS 2016
- PDF status: Metadata-only in current environment.
- Role: General reference-count bug detector based on inconsistent path-pair checking and evaluated on the Linux kernel.
- Relation to PyCGuard: RID provides a relevant inconsistency-checking technique but is not a Python/C-specific analyzer and does not model interpreter API semantics.
- Baseline status: Not included in quantitative RQ2 because its target and defect specification differ from Python/C API obligation checking; PyRefcon compares only with aggregate data from the RID paper.

#### Python/C API Empirical Study

- Citation key: `hu2020pythoncapi`
- Venue: SANER 2020
- PDF status: Metadata-only in current environment.
- Role: Empirical study of Python/C API evolution, usage statistics, and bug patterns.
- Relation to PyCGuard: Motivation and taxonomy support, not a detector.

#### Multilanguage Static Analysis of Python and C Extensions

- Citation key: `monat2021multilanguage`
- Venue: SAS 2021
- PDF status: Metadata-only in current environment.
- Role: Static analysis across Python and native C extension code.
- Relation to PyCGuard: Cross-language analysis target overlaps, but PyCGuard focuses on native extension safety obligations and LLM discharge checking.

#### PieProf

- Citation key: `pieprof2021`
- Venue: FSE 2021
- PDF status: Metadata-only in current environment.
- Role: Python/native library boundary analysis for performance.
- Relation to PyCGuard: Same boundary domain, different defect class. PieProf targets interaction inefficiency; PyCGuard targets safety bugs.

#### JNI and JavaScript Binding Analyses

- Citation keys: `kondoh2008jni`, `li2009jni`, `li2014jniexception`, `brown2017jsbindings`, `degenbaev2018crosscomponentgc`
- Venues: ISSTA 2008, CCS 2009, SCP 2014, IEEE S&P 2017, OOPSLA 2018
- PDF status: Metadata-only in current environment.
- Role: Foundational cross-language FFI/native binding work.
- Relation to PyCGuard: These works show that runtime/native boundaries impose API protocols around references, exceptions, object lifetimes, and resource ownership. PyCGuard brings this line of work to Python native extensions with a broader obligation taxonomy and LLM-based discharge reasoning.

## Group 2: LLM-Based Bug Detection

This group positions PyCGuard as a structured hybrid system: static analysis identifies obligation instances and prunes candidates, while an LLM performs path-sensitive semantic checking against explicit discharge conditions.

### Recent CCF-A or Top-Tier Seeds

#### RFCAudit

- Citation key: `rfcaudit2025`
- Venue: ASE 2025
- Local PDF: `papers/rfcaudit.pdf`
- Role: Recent CCF-A seed for LLM agent auditing against natural-language specifications.
- Summary: Uses indexing and detection agents to compare protocol implementations with RFC specifications.
- Difference from PyCGuard: RFCAudit checks protocol conformance. PyCGuard checks Python/C API safety obligations derived from documentation and real bugs.

#### Validating Network Protocol Parsers with Traceable RFC Document Interpretation

- Citation key: `rfcparser2025`
- Venue: ISSTA 2025
- PDF status: Metadata-only in current environment.
- Role: Recent CCF-A seed for specification-guided protocol implementation checking.
- Relation to PyCGuard: Similar in using natural-language specifications to guide code checking. PyCGuard differs by converting API safety rules into structured obligations and discharge conditions, then applying separate static pruning patterns.

#### Safe4U

- Citation key: `safe4u2025`
- Venue: ISSTA 2025
- Local PDF: `papers/safe4u.pdf`
- Role: Conceptually closest LLM contract-checking work.
- Summary: Decomposes Rust unsafe API safety contracts and checks whether safe wrappers guarantee them.
- Difference from PyCGuard: Safe4U checks Rust safe encapsulations of unsafe calls. PyCGuard checks Python/C native extension functions and uses static pattern-based pruning before LLM path-sensitive checking.

#### STaint

- Citation key: `staint2025`
- Venue: ASE 2025
- PDF status: Metadata-only in current environment.
- Role: Recent CCF-A LLM-assisted static taint analysis.
- Relation to PyCGuard: Both integrate LLMs with static analysis for bug detection. STaint focuses on second-order web vulnerabilities; PyCGuard focuses on Python/C API obligation violations.

#### KNighter

- Citation key: `knighter2025`
- Venue: SOSP 2025
- PDF status: Metadata-only in current environment.
- Role: Latest CCF-A systems seed using LLMs to synthesize static checkers.
- Difference from PyCGuard: KNighter uses LLMs to generate reusable static checkers. PyCGuard uses a manually derived obligation taxonomy and asks the LLM to reason about candidate paths.

#### LLM-Powered Static Binary Taint Analysis

- Citation key: `latte2025`
- Venue: TOSEM 2025
- PDF status: Metadata-only in current environment.
- Role: Top-tier LLM-assisted program analysis for binary taint.
- Relation to PyCGuard: Shares the goal of using LLMs to help hard program-analysis tasks. PyCGuard is source-level and obligation-guided.

#### Enhancing Static Analysis for Practical Bug Detection

- Citation key: `li2024llmstatic`
- Venue: OOPSLA 2024
- PDF status: Metadata-only in current environment.
- Role: CCF-A seed for LLM-integrated static bug detection.
- Relation to PyCGuard: Directly relevant hybrid static-analysis/LLM design. PyCGuard contributes domain-specific Python/C API obligation checking rather than general bug pattern enhancement.

### Additional LLM Program-Reasoning Foundations

#### IRIS

- Citation key: `iris2024`
- Venue: ICLR 2025
- PDF status: Metadata-only in current environment.
- Role: Whole-repository LLM-assisted vulnerability detection.
- Relation to PyCGuard: Similar in combining LLM reasoning with static facts, but PyCGuard narrows the reasoning task to obligation creation and discharge.

#### LLMDFA

- Citation key: `llmdfa2024`
- Venue: NeurIPS 2024
- PDF status: Metadata-only in current environment.
- Role: LLM-based customizable dataflow analysis.
- Relation to PyCGuard: Supports the idea that LLMs can help with program reasoning, but PyCGuard does not rely on unconstrained LLM dataflow inference.

#### Data-Flow Sanitization for LLM Bug Detection

- Citation key: `wang2024dataflowbug`
- Venue: EMNLP Findings 2024
- PDF status: Metadata-only in current environment.
- Role: Reduces hallucinations in LLM bug detection using explicit data-flow paths.
- Relation to PyCGuard: Motivates constraining LLM outputs with structured program facts. PyCGuard constrains LLM reasoning using trigger variables, discharge conditions, and candidate obligations.

## Suggested Related Work Shape

### Native Extension and Boundary Analysis

Start with Python/C-specific work:

1. Pungi and PyRefcon as direct reference-counting detectors.
2. Python/C API empirical work and multilanguage Python/C analysis.
3. PieProf as Python/native boundary work outside safety.

Then broaden to native boundary and unsafe analysis:

1. JNI and JavaScript bindings as classic cross-language API protocol checking.
2. Rust unsafe studies and detectors as recent CCF-A evidence that safety obligations around unsafe/native boundaries remain active research.

Key contrast sentence:

PyCGuard differs from prior native-boundary analyses by treating Python/C API requirements as dischargeable safety obligations across multiple bug classes, rather than focusing on one interface, one language pair, or one defect family.

### LLM-Based Bug Detection

Start with the closest contract/spec checking works:

1. Safe4U for unsafe API contracts.
2. RFCAudit and RFC parser validation for natural-language specification conformance.

Then discuss broader hybrid LLM/static-analysis systems:

1. OOPSLA 2024 LLM-integrated static analysis, STaint, LATTE, KNighter, IRIS, LLMDFA.
2. Data-flow sanitization as hallucination mitigation.

Key contrast sentence:

PyCGuard uses LLMs only after static candidate generation and pattern-based pruning, and asks the model to answer a constrained discharge question rather than to discover arbitrary bugs from unconstrained code context.

## Current PDF Inventory

The current repository contains exactly three paper PDFs:

- `papers/pyrefcon_ase2023.pdf`
- `papers/rfcaudit.pdf`
- `papers/safe4u.pdf`

### Metadata-Only But Important

- Native/boundary: `pungi2014`, `hu2020pythoncapi`, `monat2021multilanguage`, `kondoh2008jni`, `li2009jni`, `li2014jniexception`, `brown2017jsbindings`, `degenbaev2018crosscomponentgc`, `unsafeachilles2024`, `rustsafetypldi2020`, `mirchecker2021`, `rustsafetytse2024`, `unsaferustoopsla2020`, `dontpanic2025`.
- LLM: `li2024llmstatic`, `staint2025`, `rfcparser2025`.
