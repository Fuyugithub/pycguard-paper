# Citation Audit

Audit date: 2026-06-27.

The manuscript cites 50 unique references and `references.bib` contains exactly
the same 50 keys. Metadata was not generated from memory. All 43 DOI-bearing
entries were resolved through Crossref and checked for title, author order,
publication year, venue, volume/issue, and pages. The seven entries without a
DOI were checked on official publication or documentation pages. RustBelt's
Crossref deposit uses its December 2017 online date; the manuscript follows the
formal PACMPL 2 (POPL) citation year, 2018, as confirmed by DBLP.

## Python/C and Resource Analysis

| Key | Verification source | Manuscript claim supported |
|---|---|---|
| `pythoncapi` | Python Software Foundation, https://docs.python.org/3.14/c-api/ | The Python/C API specifies ownership, exception, resource, and runtime rules. |
| `hu2020pythoncapi` | DOI `10.1109/SANER48275.2020.9054835` | Empirical characterization of Python/C API evolution, usage, and bug patterns. |
| `pungi2014` | DOI `10.1007/978-3-662-44202-9_4` | Affine analysis of Python/C reference-count errors. |
| `pyrefcon2023` | Local paper and DOI `10.1109/ASE56229.2023.00198` | Lifecycle and reference-count analysis of Python native code. |
| `cpychecker` | Official gcc-python-plugin documentation, https://gcc-python-plugin.readthedocs.io/en/latest/cpychecker.html | Path-based abstract reference-count checking for CPython extensions. |
| `monat2021multilanguage` | DOI `10.1007/978-3-030-88806-0_16` | Multilanguage static analysis of Python with native C extensions. |
| `pieprof2021` | DOI `10.1145/3468264.3468541` | Diagnosis of inefficient Python/native-library interactions. |
| `jinn2010` | DOI `10.1145/1806596.1806601` | State-machine-based dynamic FFI checkers for JNI and Python/C rules. |
| `lal2010refcount` | DOI `10.1016/j.ipl.2010.08.003` | Reference-count analysis with shallow aliasing. |
| `referee2009` | DOI `10.1007/978-3-642-00768-2_30` | Compositional verification of reference-counting implementations. |
| `rid2016` | DOI `10.1145/2872362.2872389` | Inconsistent path-pair checking for reference-count bugs. |
| `cid2021` | USENIX official page, https://www.usenix.org/conference/usenixsecurity21/presentation/tan | Caller- and condition-level consistency checking for kernel refcounts. |
| `smoke2019` | DOI `10.1109/ICSE.2019.00025` | Scalable path-sensitive memory-leak detection. |
| `sinkfinder2020` | DOI `10.1145/3368089.3409678` | Inference of resource-relevant function pairs from seeds. |
| `smartpointer2021` | DOI `10.1109/ASE51524.2021.9678836` | Tracking C++ smart-pointer heap protocols to find memory bugs. |
| `hero2021` | USENIX official page, https://www.usenix.org/conference/usenixsecurity21/presentation/wu-qiushi | Function-pair-based detection of disordered cleanup and error handling. |

## Cross-Language Interface Safety

| Key | Verification source | Manuscript claim supported |
|---|---|---|
| `kondoh2008jni` | DOI `10.1145/1390630.1390645` | Resource and typestate bug detection in JNI programs. |
| `li2009jni` | DOI `10.1145/1653662.1653716` | Bug finding on exceptional JNI paths. |
| `li2014jniexception` | DOI `10.1016/j.scico.2014.01.018` | Interprocedural exception analysis for JNI. |
| `jet2011` | DOI `10.1145/2048066.2048095` | Coarse pruning followed by fine-grained JNI exception checking. |
| `furr2008ffi` | DOI `10.1145/1377492.1377493` | Type inference and safety checking across OCaml/C and Java/C FFIs. |
| `brown2017jsbindings` | DOI `10.1109/SP.2017.68` | Type and lifetime bug analysis for JavaScript bindings. |
| `degenbaev2018crosscomponentgc` | DOI `10.1145/3276521` | Garbage collection across managed and native components. |
| `charon2025` | DOI `10.1109/EUROSP63326.2025.00018` | Polyglot vulnerability-flow analysis for scripting-language native extensions. |
| `rustbelt2018` | DOI `10.1145/3158154`; DBLP formal record | Semantic foundations for unsafe Rust abstractions. |
| `unsaferustoopsla2020` | DOI `10.1145/3428204` | Empirical study of how programmers use unsafe Rust. |
| `rustsafetypldi2020` | DOI `10.1145/3385412.3386036` | Memory- and thread-safety practices in real Rust programs. |
| `unsafeachilles2024` | DOI `10.1145/3597503.3639136` | Empirical safety requirements of unsafe Rust programming. |
| `rudra2021` | DOI `10.1145/3477132.3483570` | Ecosystem-scale detection of unsafe Rust memory-safety bugs. |
| `mirchecker2021` | DOI `10.1145/3460120.3484541` | Static bug detection over Rust MIR. |
| `safedrop2023` | DOI `10.1145/3542948` | Data-flow detection of Rust deallocation bugs. |
| `rustsafetytse2024` | DOI `10.1109/TSE.2024.3380393` | Understanding and detecting real-world Rust safety issues. |
| `dontpanic2025` | DOI `10.1145/3719027.3765142` | Bugs hidden behind Rust runtime safety checks. |

## LLM-Assisted Bug Detection

| Key | Verification source | Manuscript claim supported |
|---|---|---|
| `chakraborty2022deepvuln` | DOI `10.1109/TSE.2021.3087402` | Limitations and generalization concerns in learned vulnerability detection. |
| `steenhoek2023vuln` | DOI `10.1109/ICSE48619.2023.00188` | Empirical comparison of deep vulnerability-detection models. |
| `ullah2024llmvuln` | DOI `10.1109/SP54263.2024.00210` | Broad evaluation of LLM vulnerability identification and reasoning reliability. |
| `gptscan2024` | DOI `10.1145/3597503.3639117` | Combining GPT and program analysis for smart-contract logic bugs. |
| `li2024llmstatic` | DOI `10.1145/3649828` | LLM validation of conventional static-analysis warnings. |
| `iris2025` | ICLR 2025 OpenReview, https://openreview.net/forum?id=9LdJDU7E91 | LLM-inferred taint specifications and contextual whole-repository analysis. |
| `staint2025` | DOI `10.1109/ASE63991.2025.00347` | LLM-assisted bidirectional taint analysis for second-order PHP vulnerabilities. |
| `latte2025` | DOI `10.1145/3711816` | LLM-guided static binary taint analysis. |
| `wang2024dataflowbug` | DOI `10.18653/v1/2024.findings-emnlp.217` | Constraining LLM bug judgments with data-flow facts. |
| `llmdfa2024` | NeurIPS official proceedings and DOI `10.52202/079017-4181` | Decomposed LLM data-flow analysis with external path-feasibility validation. |
| `dainfer2024` | DOI `10.1145/3660816` | Inferring API aliasing specifications from documentation. |
| `knighter2025` | DOI `10.1145/3731569.3764827` | Synthesizing static checkers from historical patches with LLMs. |
| `safe4u2025` | Local paper and DOI `10.1145/3728890` | Contract decomposition and checking of safe Rust wrappers around unsafe calls. |
| `rfcaudit2025` | Local paper and DOI `10.1109/ASE63991.2025.00105` | Agent-based auditing of implementations against RFC specifications. |
| `rfcparser2025` | DOI `10.1145/3728955` | Traceable interpretation of RFC text for protocol-parser validation. |

## Disclosure References

| Key | Verification source | Manuscript claim supported |
|---|---|---|
| `openaicodex` | Official OpenAI documentation, https://developers.openai.com/codex | Disclosure of the drafting and language-editing assistant. |
| `openaiimage` | Official OpenAI documentation, https://developers.openai.com/api/docs/guides/image-generation | Disclosure of generated base artwork for Figure 2. |

## Consistency Checks

- Unique citation keys in section sources: 50.
- Unique BibTeX keys: 50.
- Cited keys missing from BibTeX: none.
- BibTeX entries not cited in the manuscript: none.
- Duplicate citation keys, DOI values, or normalized titles: none.
- Crossref DOI resolutions: 43 of 43 successful.
- Non-DOI official-page resolutions: 7 of 7 successful.
- BibTeX completed without warnings; the final LaTeX pass has no undefined
  citations or references.
- The compiled manuscript is 12 pages; conclusion and references begin on page
  10, so non-reference content does not exceed the 10-page research-track
  limit.

The local PDFs for PyRefcon, Safe4U, and RFCAudit were also inspected as
structural exemplars. Their text was not copied.
