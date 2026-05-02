---
name: paper-review
description: Review academic papers, manuscripts, preprints, or drafts in the style of a conference peer review, with blunt evidence-based critique and concrete next revision steps. Use when the user asks for a paper review, referee report, conference-style review, acceptance recommendation, weakness analysis, rebuttal preparation, or no-sugarcoating feedback on a paper.
license: MIT
---

# Paper Review

## Overview

Review the paper as a serious academic reviewer, not as a copyeditor or cheerleader. Surface the decision-relevant weaknesses plainly, assign defensible scores, and end with the smallest set of revisions most likely to improve the paper's fate.

## Workflow

1. Identify the target venue or field if provided. If absent, use a general conference-review standard and state that assumption.
2. Read the manuscript, figures, tables, appendix, and prompt-specific constraints before judging. If the paper is unavailable or only an abstract is provided, review only the available evidence and state the limitation.
3. Evaluate the paper against its own claims first, then against field norms. Do not invent missing experiments, citations, or results.
4. Write in the user's language by default. If the paper and user language differ, prioritize the user's language unless they request otherwise.
5. Be blunt but professional: criticize claims, evidence, method, writing, and positioning; never attack the authors.

## Review Criteria

Assess these dimensions when relevant:

- **Summary**: What the paper claims to contribute, in neutral terms.
- **Novelty**: Whether the idea, formulation, dataset, theorem, system, or empirical finding is meaningfully new.
- **Significance**: Whether the contribution matters to the target community.
- **Soundness**: Whether the method, assumptions, proofs, experiments, statistics, and conclusions are justified.
- **Evidence**: Whether baselines, ablations, datasets, evaluation metrics, error analysis, and qualitative evidence support the claims.
- **Clarity**: Whether the problem, method, limitations, and results are understandable and well organized.
- **Reproducibility**: Whether implementation details, data, code, hyperparameters, prompts, environment, and evaluation protocol are sufficient.
- **Ethics and limitations**: Whether risks, misuse, consent, data provenance, privacy, bias, and deployment caveats are handled honestly.
- **Positioning**: Whether related work is accurate and the paper distinguishes itself from the closest prior work.

## Output Format

Use this structure unless the user requests a different format:

```markdown
## Verdict
Recommendation: [Strong Accept / Accept / Weak Accept / Borderline / Weak Reject / Reject / Strong Reject]
Confidence: [Low / Medium / High]

One-sentence rationale: ...

## Summary
...

## Strengths
- ...

## Major Weaknesses
- ...

## Detailed Comments
- ...

## Scores
- Novelty: [1-5] ...
- Significance: [1-5] ...
- Soundness: [1-5] ...
- Evidence: [1-5] ...
- Clarity: [1-5] ...
- Reproducibility: [1-5] ...

## Required Revisions
- P0: ...
- P1: ...
- P2: ...

## Next Actions
1. ...
2. ...
3. ...
```

## Scoring Guidance

- Use the full scale. 5: Exceptional/Award-quality; 4: Strong contribution; 3: Adequate but not compelling; 2: Likely below acceptance quality; 1: Fundamental problem.
- Align the recommendation with the major weaknesses. Do not give a positive recommendation if the evidence does not support the core claim.
- If the paper is interesting but not ready, say so directly and identify the blocker.
- Distinguish "fixable in revision" from "requires a different paper." This is often the most useful part of the review.

## Revision Plan Rules

Make revision advice specific enough to act on:

- Start with P0 changes that must be fixed before submission or rebuttal.
- Tie each action to a section, experiment, proof, table, figure, or claim when possible.
- Explain why the action changes the review outcome.
- Prefer a short, high-leverage plan over a long wishlist.
- Include rebuttal strategy only when the user asks or when the context is a submitted paper under review.

## External Knowledge

Do not hallucinate related work or venue rules. If novelty or positioning depends on current literature and the user asks for that check, search authoritative sources and cite them. Otherwise, mark uncertain comparisons as "needs verification against closest prior work."
