# AI Use Case Prioritization Framework

Systematically prioritize AI use cases using an Impact vs. Feasibility matrix with weighted scoring.

---

## Step 1: List Candidate Use Cases

| # | Use Case | Business Area | Sponsor |
|---|----------|--------------|---------|
| 1 | | | |
| 2 | | | |
| 3 | | | |

---

## Step 2: Score Each Use Case

### Impact Score (1-5)

| Factor | Weight | Description |
|--------|--------|-------------|
| **Revenue Impact** | 25% | Direct revenue generation or cost savings |
| **User Value** | 25% | Improvement to user experience or outcomes |
| **Strategic Alignment** | 25% | Alignment with company strategy and AI roadmap |
| **Scale of Effect** | 25% | Number of users/processes affected |

### Feasibility Score (1-5)

| Factor | Weight | Description |
|--------|--------|-------------|
| **Data Availability** | 25% | Is training/inference data available, clean, and sufficient? |
| **Technical Complexity** | 25% | Can existing infra support it? Is the AI problem well-understood? |
| **Time to Value** | 25% | How quickly can an MVP be delivered? |
| **Risk Level** | 25% | Regulatory, reputational, and technical risk |

---

## Step 3: Impact vs. Feasibility Matrix

```
HIGH IMPACT
     │
     │  ★ QUICK WINS        🚀 STRATEGIC BETS
     │  (High Impact,       (High Impact,
     │   High Feasibility)   Low Feasibility)
     │
─────┼─────────────────────────────────
     │
     │  ✅ LOW HANGING       ⚠️ RECONSIDER
     │  (Low Impact,        (Low Impact,
     │   High Feasibility)   Low Feasibility)
     │
     └──────────────────────── HIGH FEASIBILITY
```

### Priority Order:
1. **★ Quick Wins** — Do first. High impact, easy to execute.
2. **🚀 Strategic Bets** — Plan for these. High impact but need investment.
3. **✅ Low Hanging** — Fill-in work. Easy wins that keep momentum.
4. **⚠️ Reconsider** — Deprioritize or drop. Low ROI.

---

## Step 4: Detailed Scoring Template

### Use Case: _______________

**Impact Assessment:**

| Factor | Score (1-5) | Weight | Weighted | Evidence/Notes |
|--------|-------------|--------|----------|----------------|
| Revenue Impact | | 25% | | |
| User Value | | 25% | | |
| Strategic Alignment | | 25% | | |
| Scale of Effect | | 25% | | |
| **Impact Total** | | | **___** | |

**Feasibility Assessment:**

| Factor | Score (1-5) | Weight | Weighted | Evidence/Notes |
|--------|-------------|--------|----------|----------------|
| Data Availability | | 25% | | |
| Technical Complexity | | 25% | | |
| Time to Value | | 25% | | |
| Risk Level | | 25% | | |
| **Feasibility Total** | | | **___** | |

**Combined Score:** Impact ___ × Feasibility ___ = **___**

---

## Step 5: Ranked Priority List

| Rank | Use Case | Impact | Feasibility | Combined | Quadrant | Recommendation |
|------|----------|--------|-------------|----------|----------|---------------|
| 1 | | | | | | |
| 2 | | | | | | |
| 3 | | | | | | |

---

## Go/No-Go Checklist

Before committing to a use case, verify:

- [ ] Clear success metrics defined
- [ ] Data pipeline identified and accessible
- [ ] Model approach validated (or prototype path clear)
- [ ] Stakeholder buy-in secured
- [ ] Ethical/bias risks assessed
- [ ] Failure mode and fallback plan documented
- [ ] Human-in-the-loop requirements defined
- [ ] Estimated timeline and resources documented
