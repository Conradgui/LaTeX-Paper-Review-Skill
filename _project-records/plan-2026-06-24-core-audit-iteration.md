# Core Audit Iteration Plan

## Goal

Improve `$paper-review` without changing its positioning as a general academic manuscript review skill.

## Scope

- Add a required Core Manuscript Audit Protocol for all manuscripts.
- Add Recheck / Delta Review for revision loops.
- Add empirical-paper and claim-strength references as conditional enhancement modules.
- Add a concise action plan table to the default output contract.
- Add Chinese overclaiming candidate rules to the proofing scan script.
- Improve `agents/openai.yaml` prompt specificity.

## Explicit Non-Goals

- Do not add Undergraduate Thesis Mode.
- Do not add table/CSV consistency automation.
- Do not add bibliography verification helper.
- Do not change `$paper-review` positioning.
- Do not change `$latex-paper-review` unless a shared-script regression requires it.

## Validation Table

1. Core audit exists -> Verify `Core Manuscript Audit Protocol` appears in `paper-review/SKILL.md`.
2. Delta review exists -> Verify `Recheck / Delta Review` appears in `paper-review/SKILL.md`.
3. Action table exists -> Verify `修改行动表` appears and `预计工作量` does not.
4. References are routed -> Verify `empirical-paper-audit.md` and `claim-strength-calibration.md` are referenced conditionally from `SKILL.md`.
5. Metadata validates -> Run `quick_validate.py paper-review` and `quick_validate.py latex-paper-review`.
6. Script compiles -> Run `py_compile` with `PYTHONPYCACHEPREFIX`.
7. Script behavior -> Verify old English/math hits and new Chinese overclaiming hits.

## Execution Evidence

- Added `Core Manuscript Audit Protocol` to `paper-review/SKILL.md`.
- Added `Recheck / Delta Review` to support revision-loop audits.
- Added conditional references: `empirical-paper-audit.md` and `claim-strength-calibration.md`.
- Added the default `修改行动表` without `预计工作量`.
- Added Chinese overclaiming candidate rules to `proofing_scan.py`.
- Updated `agents/openai.yaml` with a more specific evidence-focused default prompt.
- Verified `quick_validate.py paper-review` returns `Skill is valid!`.
- Verified `quick_validate.py latex-paper-review` returns `Skill is valid!`.
- Verified `paper-review/scripts/proofing_scan.py` compiles with `PYTHONPYCACHEPREFIX=/tmp/paper-review-pycache`.
- Verified the old English/math proofing sample still catches `PUNC_DOUBLE_COMMA`, `CAP_JAVASCRIPT`, and `ARCTAN_DIV`.
- Verified the Chinese risk sample catches `ZH_OVERCLAIM_CAUSAL`, `ZH_OVERCLAIM_STABILITY`, `ZH_OVERCLAIM_SCOPE`, and `ZH_MIXED_EVIDENCE_LANGUAGE`.
