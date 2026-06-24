---
name: latex-paper-review
description: Use when reviewing scientific and technical manuscripts written in LaTeX; checking LaTeX source, equations, derivations, notation, appendix formulas, cross-references, citation keys, compileability, implementation ambiguity, unsupported claims, writing issues, or generating a LaTeX review file.
---

# LaTeX Paper Review

## Overview

Produce one concrete, compileable LaTeX review file. Prioritize scientific and technical correctness over prose polish, then add bounded editorial findings that materially improve clarity or professionalism.

## Default Behavior

1. Read the full manuscript before writing findings.
2. Use extra thinking by default, but do not narrate the reasoning process.
3. Write a standalone LaTeX review to `paper-reviews/review-YYYY-MM-DD-HHMMSS.tex`, unless the user gives a different path.
4. Match the user's requested output language; default to Simplified Chinese (简体中文) unless specified otherwise.
5. Preserve equations, symbols, citation keys, and quoted manuscript snippets exactly when needed for technical accuracy.
6. Do not require a fixed review template or section layout unless the user asks for one.
7. Create a new timestamped review file by default. Only update an existing review file if the user explicitly asks.
8. Compiling to PDF is optional unless the user asks for it or compilation is needed to validate the LaTeX.

## Review Priority

Spend attention in this order:

1. equations, derivations, notation, units, definitions, algorithms, and implementation-facing formulas
2. consistency between claims, evidence, figures, tables, captions, appendices, and conclusions
3. editorial quality, especially high-confidence issues that are cheap to verify and fix

Do not let editorial findings crowd out technical findings. Treat `proofread`, `flag typos`, `grammar`, `writing quality`, and similar wording as a request to spend more budget on the editorial sweep after the technical audit.

## Workflow

1. Read the manuscript to understand the main claims, evidence, equations, appendices, and structure.
2. Build a structured internal map of the manuscript:
   - **Research Question Map**: Identify core research questions, primary claims, and intended contributions.
   - **Construct / Variable / Parameter Map**: Identify independent/dependent variables, mathematical parameters, system components, or theoretical constructs (relevant to the paper type).
   - **Relationship Map**: Identify hypothesized directions, logical dependencies, mathematical relationships, or system interactions.
   - **Method Fit Assessment**: Evaluate whether the selected research design or proof strategy logically answers the research questions, and whether conclusion boundaries match the evidence.
3. Trace the Evidence Chain for identified claims: Check the path `Claim` -> `Evidence` -> `Figure/Table` -> `Method` -> `Citation` -> `Appendix` (if applicable) for inconsistencies, missing links, or overreaching assertions.
4. Identify the highest-risk technical content: key equations, symbol definitions, quantitative claims, coordinate transformations, appendix formulas, and implementation-facing expressions.
5. Perform the technical audit, checking specific methodological validity dimensions (such as `selection bias`, `endogeneity`, `validity`, and `boundary conditions` where applicable).
6. Re-check the findings and classify them by severity and certainty before presenting.
7. Perform a bounded editorial pass after the technical audit.
8. Run `scripts/proofing_scan.py` when code execution is available; otherwise do the manual pattern scan in `Final Proofing Sweep`.
9. Spot-check proofing-scan candidates before presenting them as issues.
10. Write the review file.

For long papers, inspect sections in chunks when helpful, but do not force a ritual that does not improve the review.

## Technical Audit

Always check for:

- unsupported or overstated claims
- conclusions that do not follow from the reported results
- inconsistency between text, equations, figures, tables, captions, and appendices
- undefined symbols or symbols used before definition
- notation drift between sections or between main text and appendix
- unit errors or dimensional inconsistencies
- arithmetic or quantitative inconsistencies
- sign errors, missing factors, normalization ambiguity, or missing case distinctions
- inverse-trig, branch, or quadrant ambiguity, such as `arctan(x/y)` where `atan2` or an explicit branch convention may be needed
- coordinate-system ambiguity, including misuse of initial, final, source, observer, local, global, polar, or azimuthal
- mismatch between mathematical definitions and appendix or code-facing formulas
- missing assumptions, qualifiers, or uncertainty that materially affect interpretation
- factual statements that appear unsupported or likely incorrect

### Notation And Terminology Drift

For frequently used symbols, especially subscripted or superscripted angles, radii, and frame labels, record:

- where the symbol is first defined
- the verbal descriptor attached to it
- later descriptor variants and whether they conflict

Flag conflicts where the same symbol is described with inconsistent role or temporal adjectives, such as initial/final, source/observer, or emission/reception.

### Severity And Certainty Classification

For each finding, you must determine both its severity and certainty:

#### Severity Levels
- **Critical Issues**: Factual, logical, or methodological issues that threaten the primary validity of the manuscript (e.g., unsupported core claims, missing identification strategy, undefined key variables, contradictory results).
- **Major Issues**: Significant problems that weaken confidence in the findings (e.g., measurement ambiguity, incomplete methodological justification, weak evidence chain).
- **Minor Issues**: Localized errors that affect clarity, consistency, or readability (e.g., terminology inconsistency, local figure-text mismatches, formatting problems).
- **Writing Suggestions**: Non-essential suggestions to improve flow, grammar, or phrasing.

#### Certainty Labels
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

## Coverage Requirement

If the manuscript contains equations, derivations, appendices, or implementation formulas, inspect them explicitly. Do not finish the review without checking branch conventions, sign conventions, symbol definitions, dimensional consistency, and implementation ambiguity.

If no concrete technical issues are found, say explicitly in the review file that these checks were performed and no concrete issues were identified.

## Editorial Review

Include meaningful editorial issues that affect meaning, create technical ambiguity, or noticeably reduce professionalism.

Look for:

- typos, spelling, grammar, and punctuation issues
- awkward, vague, or ambiguous phrasing
- sentences that obscure a technical point
- broken references or citation mismatches
- inconsistent terminology or notation
- malformed derivation or equation-adjacent prose
- duplicated punctuation, malformed quoted titles, inconsistent capitalization, and obvious citation-format glitches in references
- capitalization or styling mistakes in proper names, product names, languages, or branded terms

Do not spend space on subjective line editing or style preferences. Include only a bounded number of minor proofing items unless the user explicitly asks for exhaustive proofreading.

## Final Proofing Sweep

Run:

```bash
python3 scripts/proofing_scan.py <path-to-pdf-or-text> --max-hits 80
```

Use hits as candidates, then spot-check each before including it. If code execution is unavailable, manually scan for:

- duplicated punctuation and spacing artifacts: `, ,`, `,,`, `..`, `" , ,`, `" ,`
- semicolon capitalization anomalies: `;` followed by a capital letter where the second clause is not a new sentence or proper noun
- malformed equation-adjacent grammar frames: `into ... into ...`, `with ... into ...`, `plugging ... into together ...`
- programming or product-name capitalization: JavaScript, iOS, iPhone
- quadrant or branch ambiguity patterns, especially `arctan(x/y)` expressions

Before finishing, check bibliography/reference hygiene and dense derivation prose near equations.

## Output Contract

Write LaTeX only in the review file. Use minimal standalone scaffolding that can compile.
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

Default report structure:
- Overall assessment of manuscript quality (整体评估)
- Detailed findings grouped by **Severity** (from Critical down to Minor/Suggestions)
- High-priority fixes (高优先级修改建议)
- Items needing external verification (需核对/确认事项)

## Useful Reference

Read `references/review-rubric.md` when a broader checklist would help, especially for long manuscripts or revisions after local edits.
