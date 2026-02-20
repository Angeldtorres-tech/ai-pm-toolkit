# Prompt Library Template

A structured approach to building, versioning, and maintaining a prompt library for your team or organization.

---

## Library Structure

```
prompts/
├── categories/
│   ├── content-generation/
│   │   ├── blog-post-draft.md
│   │   └── social-media-copy.md
│   ├── analysis/
│   │   ├── sentiment-analysis.md
│   │   └── data-summarization.md
│   ├── code/
│   │   ├── code-review.md
│   │   └── bug-diagnosis.md
│   └── internal/
│       ├── meeting-summary.md
│       └── email-draft.md
├── CHANGELOG.md
└── README.md
```

---

## Prompt Card Template

Use this template for every prompt in your library:

```markdown
# [Prompt Name]

**ID:** PROMPT-[CATEGORY]-[NUMBER]  
**Version:** 1.0  
**Author:** [Name]  
**Last Updated:** [Date]  
**Model:** [Target model, e.g., Claude Sonnet 4, GPT-4o]  
**Status:** Draft | Testing | Production | Deprecated

## Purpose
[One-line description of what this prompt does]

## Prompt

```
[The actual prompt text, including system prompt and user prompt]
```

## Variables
| Variable | Type | Description | Example |
|----------|------|-------------|---------|
| {{input}} | string | | |

## Example Input
[Sample input]

## Expected Output
[What good output looks like]

## Performance Metrics
| Metric | Target | Actual | Date Tested |
|--------|--------|--------|-------------|
| Accuracy | | | |
| Relevance | | | |
| Tone Match | | | |

## Notes
- Known limitations
- Edge cases
- Tips for best results
```

---

## Versioning Convention

```
MAJOR.MINOR

MAJOR = Significant prompt restructure or model change
MINOR = Tweaks, variable changes, instruction refinement
```

**Example changelog entry:**
```
## v1.2 — 2026-02-15
- Added few-shot examples for edge cases
- Reduced hallucination by adding "only use provided context" instruction
- Tested on Claude Sonnet 4 (accuracy: 92% → 96%)
```

---

## Testing Framework

### Before promoting a prompt to Production:

1. **Baseline Test** — Run 10+ representative inputs, score outputs
2. **Edge Case Test** — Run 5+ adversarial/unusual inputs
3. **Regression Test** — Compare against previous version outputs
4. **Cross-Model Test** — Verify performance if model changes

### Scoring Rubric

| Score | Quality |
|-------|---------|
| 5 | Perfect — No edits needed |
| 4 | Good — Minor polish only |
| 3 | Acceptable — Usable with edits |
| 2 | Poor — Major revision needed |
| 1 | Failed — Unusable output |

**Promotion threshold:** Average score ≥ 4.0 across test set

---

## Performance Tracking

Track these metrics monthly for production prompts:

| Prompt ID | Month | Uses | Avg Score | Failure Rate | Cost/Run | Notes |
|-----------|-------|------|-----------|-------------|----------|-------|
| | | | | | | |

### Red Flags to Watch
- Failure rate > 10%
- Average score dropping below 3.5
- Cost per run increasing unexpectedly
- User complaints about specific prompt category

---

## Governance

- **Review cadence:** Monthly review of all production prompts
- **Deprecation:** Mark unused prompts (no runs in 60 days) for review
- **Access control:** Define who can promote prompts to production
- **Audit trail:** All changes logged in CHANGELOG.md with author and date
