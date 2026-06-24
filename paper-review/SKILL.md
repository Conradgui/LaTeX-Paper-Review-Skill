---
name: paper-review
description: Use when reviewing, proofreading, or auditing scientific and technical manuscripts in Markdown, DOCX, PDF, LaTeX, or plain text; checking research questions, argument logic, variables, methods, equations, evidence, figures, tables, appendices, citations, unsupported claims, writing clarity, or producing a structured manuscript review.
---

# Paper Review

## Overview

Produce a concrete, evidence-focused manuscript review. Default to a Markdown review report unless the user requests LaTeX, DOCX-ready prose, inline edits, Chinese, English, or another output format.

Prioritize research quality and internal consistency over surface-level polishing.

## Default Behavior

1. Read the full manuscript or all provided manuscript material before writing findings.
2. Preserve equations, symbols, citation keys, table/figure labels, and quoted snippets exactly when needed for accuracy.
3. Match the user's requested language; default to Simplified Chinese (简体中文) unless specified otherwise.
4. Create a new timestamped review file at `paper-reviews/review-YYYY-MM-DD-HHMMSS.md` when writing to disk, unless the user gives another path.
5. Do not modify the manuscript unless the user explicitly requests edits or a revision pass.
6. Do not require a fixed review template. Use the structure that best exposes actionable issues.
7. If the manuscript is incomplete, review what is available and state which checks are limited by missing files.

## Input Handling

Support Markdown, DOCX, PDF, LaTeX source, and plain text.

- **Markdown or plain text:** review structure, claims, citations, tables, formulas, and prose directly.
- **DOCX:** extract or inspect content with available document tools; preserve section/table context in findings.
- **PDF:** review visible content; note that source-level comments, hidden metadata, and some cross-reference checks may be limited.
- **LaTeX:** inspect source when available; for LaTeX-specific source, macro, cross-reference, and compileability concerns, prefer `$latex-paper-review` when installed.
- **Multi-file projects:** inspect the main file plus included chapters, bibliography, appendices, tables, figures/captions, and supporting notes when available.

## Manuscript Understanding Model

Before formulating any review findings, build a structured internal map covering:

- **Research Question Map**: Identify the core research questions, primary claims, and intended contributions.
- **Construct / Variable / Parameter Map**: Identify independent/dependent variables, mediators, moderators, control variables, mathematical parameters, system components, or theoretical constructs (as applicable to the manuscript type).
- **Relationship Map**: Identify hypothesized directions, causal links, logical dependencies, mathematical relationships, or system interactions.
- **Method Fit Assessment**: Evaluate whether the selected research design or method logically answers the research questions, whether the generated evidence supports the stated claims, and whether conclusion boundaries match the evidence.

Use this map to test whether the paper's claims, method, evidence, and wording align. Do not output this model in the final report unless it directly exposes an issue.

## Review Workflow

1. Identify manuscript type: empirical, theoretical/mathematical, systems/technical, review/conceptual, or mixed.
2. Build the manuscript understanding model.
3. Trace the Evidence Chain for identified claims: Check the path `Claim` -> `Evidence` -> `Figure/Table` -> `Method` -> `Citation` -> `Appendix` (if applicable) for inconsistencies, missing links, or overreaching assertions.
4. Audit the highest-risk content first: research question, method, variables/symbols, evidence, results, equations, figures/tables, appendices, and citations.
5. Re-check the findings and classify them by severity and certainty before presenting.
6. Run `scripts/proofing_scan.py` when code execution is available and the input is PDF, DOCX, Markdown, or text-like; otherwise do the manual proofing scan below.
7. Perform a bounded editorial pass after the technical and evidence audit.
8. Write the review report with findings structured by severity, prioritizing technical/factual issues.

## Review Lenses

Use these lenses as applicable:

- **Structure:** title, abstract, introduction, related work, method, results, discussion, conclusion, and appendices form a coherent path.
- **Evidence:** claims are supported by data, figures, tables, experiments, citations, derivations, or stated assumptions.
- **Method:** study design, variables, model, experiment, proof, or review method can answer the research question.
- **Expression:** terms, paragraphs, figures, tables, references, and formatting support reader comprehension.
- **Risk:** overclaiming, causal overreach, missing caveats, sample limits, external validity, unsupported novelty, or ambiguity.

## Manuscript-Type Checks

For each manuscript type, check specific methodological validity dimensions (if applicable):

- **Empirical papers**: Check variable definitions, measurement validity, model specification, table interpretation, robustness checks, causal language, causal boundary limits, selection bias, endogeneity, construct reliability, and conclusion boundaries.
- **Theoretical or mathematical papers**: Check definitions, assumptions, notation, boundary conditions, theorem/proposition statements, proof steps, dimensions, signs, branches, and symbol drift.
- **Systems or technical papers**: Check task framing, baseline fairness, evaluation metrics, implementation ambiguity, reproducibility, ablations, external validity, and claim-to-result alignment.
- **Review or conceptual papers**: Check taxonomy logic, concept boundaries, source representativeness, citation support, and whether synthesis goes beyond summary.

## Technical And Evidence Audit

Always check for:

- unsupported or overstated claims
- conclusions that do not follow from reported results
- inconsistency between text, equations, figures, tables, captions, appendices, and conclusions
- undefined variables, constructs, terms, symbols, or abbreviations
- notation or terminology drift
- unit, dimensional, arithmetic, or quantitative inconsistencies
- sign errors, missing factors, normalization ambiguity, or missing case distinctions
- inverse-trig, branch, or quadrant ambiguity, such as `arctan(x/y)` where `atan2` or an explicit branch convention may be needed
- mismatch between definitions, formulas, appendix material, and implementation-facing expressions
- missing assumptions, qualifiers, limitations, or uncertainty that materially affect interpretation
- citation/reference mismatch or citation used to support a claim it does not actually establish

## Severity And Certainty Classification

For each finding, you must determine both its severity and certainty:

### Severity Levels
- **Critical Issues**: Factual, logical, or methodological issues that threaten the primary validity of the manuscript (e.g., unsupported core claims, missing identification strategy, undefined key variables, contradictory results).
- **Major Issues**: Significant problems that weaken confidence in the findings (e.g., measurement ambiguity, incomplete methodological justification, weak evidence chain).
- **Minor Issues**: Localized errors that affect clarity, consistency, or readability (e.g., terminology inconsistency, local figure-text mismatches, formatting problems).
- **Writing Suggestions**: Non-essential suggestions to improve flow, grammar, or phrasing.

### Certainty Labels
- **确定错误 (Definite Error)**: Contradicted directly by the manuscript's own contents, math, or established logic.
- **证据不足 (Unsupported Claim)**: Stated more strongly than the manuscript's data or references justify.
- **疑似问题 (Likely Issue)**: Highly probable issue that requires authors' attention but cannot be proven definitively from the text alone.
- **需核对/确认 (Requires Verification)**: Depends on external literature or context outside the manuscript.

## External Research Policy

Do not perform external web or literature searches by default. Only perform searches when explicitly authorized by the user, when a finding cannot be assessed without external knowledge, or when the manuscript explicitly requests fact validation.

When searching, protect manuscript privacy by using anonymized queries (do not upload the title, author names, or sensitive draft text).

### Source Credibility Hierarchy
- **Tier 1 (Authoritative)**: Original cited papers, peer-reviewed journals/conferences (e.g., ACM/IEEE, Nature, MISQ, AER), official standards, standard textbooks, official documentation, academic databases (OpenAlex, PubMed, arXiv).
- **Tier 2 (Supporting)**: University lecture notes, technical manuals, high-quality reviews, preprints.
- **Tier 3 (Non-authoritative)**: General forums (StackOverflow, Reddit), commercial/marketing blogs, AI summaries without sources.
*Rule: Tier 3 sources may only serve as search leads or prompts to check further, but MUST NEVER be cited as academic evidence, consensus, or proof of error.*

### Cross-Validation Rule
When a review conclusion depends on external academic consensus, cross-check multiple authoritative (Tier 1/2) sources whenever feasible. If evidence remains uncertain, label the issue as "requires verification" rather than asserting that the manuscript is incorrect.

## Editorial Review

Editorial review is secondary. Include issues that affect meaning, technical clarity, or professionalism:

- ambiguous phrasing that changes interpretation
- inconsistent terminology
- weak transitions that obscure argument logic
- unclear figure/table descriptions
- malformed equation-adjacent prose
- broken references, duplicated punctuation, malformed titles, or obvious citation-format glitches
- high-confidence grammar, spelling, or capitalization issues

Avoid subjective line editing unless the user requests exhaustive proofreading.

## Final Proofing Sweep

Run when useful:

```bash
python3 scripts/proofing_scan.py <path-to-pdf-or-text> --max-hits 80
```

Use hits as candidates and spot-check before including them.

If code execution is unavailable, manually scan for duplicated punctuation, malformed equation-adjacent prose, product/language capitalization, citation-format glitches, and branch/quad ambiguity patterns such as `arctan(x/y)`.

## Output Contract

Generate the default report in Simplified Chinese (简体中文) unless the user requests another language.

### Anti-Generic Feedback Rules
You must NOT output low-value, vague suggestions (e.g., "增强创新性", "扩展文献", "提升贡献") that lack precise local facts or actionable advice.
If no concrete issues are identified, output "未发现明显问题" directly. Do not invent minor issues just to fill space.

### Finding Format
For every identified finding, you MUST include:
1. **Location** (e.g., section number, page, line number, or equation/figure label).
2. **Problem** (factual explanation of the issue).
3. **Why it matters** (impact on manuscript validity or readability).
4. **Suggested revision** (concrete, actionable remedy).
5. **Severity** (Critical, Major, Minor, or Writing Suggestion).
6. **Certainty** (确定错误, 证据不足, 疑似问题, or 需核对/确认).
*Note: Only output the Evidence Chain trace path if an issue or discrepancy in the chain is found.*

Default Markdown report structure:
- Concise overall assessment (整体评估)
- Detailed findings grouped by **Severity** (from Critical down to Minor/Suggestions)
- High-priority fixes (高优先级修改建议)
- Items needing external verification (需核对/确认事项)
