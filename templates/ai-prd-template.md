# AI Feature PRD Template

A Product Requirements Document template specifically designed for AI-powered features.

---

## Feature Overview

| Field | Details |
|-------|---------|
| **Feature Name** | |
| **Product Manager** | |
| **Engineering Lead** | |
| **Status** | Draft / In Review / Approved |
| **Target Release** | |
| **Last Updated** | |

---

## 1. Problem Statement

**What problem does this solve?**

**Who has this problem?** (User persona)

**Current solution / workaround:**

**Why now?** (Market timing, technical feasibility, strategic priority)

---

## 2. Goals & Success Metrics

| Metric | Current Baseline | Target | Measurement Method |
|--------|-----------------|--------|-------------------|
| | | | |

**North star metric:**

**Guardrail metrics** (things that must NOT get worse):

---

## 3. User Stories

```
As a [persona],
I want to [action],
so that [outcome].
```

---

## 4. Solution Overview

**High-level approach:**

**Why AI?** (Why is ML/AI the right approach vs. rules/heuristics?)

---

## 5. AI/ML Specifications

### Model Selection

| Consideration | Decision | Rationale |
|--------------|----------|-----------|
| **Model type** | (LLM / Classification / Regression / etc.) | |
| **Specific model** | (Claude Sonnet / GPT-4o / Fine-tuned / Custom) | |
| **Hosting** | (API / Self-hosted / Edge) | |
| **Fallback model** | | |

### Training Data

- **Data sources:**
- **Data volume:**
- **Data quality assessment:**
- **Labeling requirements:**
- **Data refresh cadence:**
- **PII/sensitive data handling:**

### Evaluation Metrics

| Metric | Minimum Threshold | Target | Measurement |
|--------|-------------------|--------|-------------|
| Accuracy | | | |
| Latency (p95) | | | |
| Cost per inference | | | |
| User satisfaction | | | |

### Prompt Engineering (if LLM-based)

- **System prompt:** [Link or summary]
- **Few-shot examples needed?**
- **Context window requirements:**
- **Output format:** (JSON / natural language / structured)

---

## 6. Failure Modes & Mitigations

| Failure Mode | Likelihood | Impact | Mitigation | Fallback |
|-------------|-----------|--------|------------|----------|
| Model hallucination | | | | |
| High latency / timeout | | | | |
| Biased output | | | | |
| Prompt injection | | | | |
| Model API outage | | | | |
| Cost spike | | | | |

---

## 7. Human Oversight

| Decision | Automation Level | Human Role |
|----------|-----------------|------------|
| | Fully Automated / Human-in-the-Loop / Human-on-the-Loop / Manual | |

**Escalation criteria:** (When does the system escalate to a human?)

**Override mechanism:** (How can a human correct the AI?)

**Audit trail:** (What gets logged for review?)

---

## 8. Privacy & Ethics

- [ ] Data handling compliant with privacy policy
- [ ] User informed they're interacting with AI
- [ ] Bias evaluation planned
- [ ] No prohibited use cases (per AI policy)
- [ ] Data not used for model training without consent

---

## 9. Technical Requirements

**API/Infrastructure:**

**Dependencies:**

**Performance:**
- Max latency:
- Throughput:
- Availability SLA:

**Cost budget:** $ ___/month

---

## 10. Launch Plan

| Phase | Scope | Duration | Success Criteria |
|-------|-------|----------|-----------------|
| Alpha | Internal team testing | | |
| Beta | % of users | | |
| GA | Full rollout | | |

**Kill criteria:** (What would cause us to roll back?)

---

## 11. Open Questions

| # | Question | Owner | Due Date | Resolution |
|---|----------|-------|----------|------------|
| 1 | | | | |

---

## Appendix

- Links to design mocks
- Technical architecture diagrams
- Competitive analysis
- Research references
