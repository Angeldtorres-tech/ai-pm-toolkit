# Product Roadmap: AI Customer Intelligence Engine

**Author:** Angel Torres | **Date:** 2026-02-24
**Contextualized for:** Marigold (Marketing Technology, 40K+ customers)

---

## Roadmap Overview

```
Phase 1: Foundation     Phase 2: Intelligence    Phase 3: Proactive     Phase 4: Autonomous
Weeks 1-4               Weeks 5-8                Weeks 9-12             Weeks 13+
─────────────────────── ──────────────────────── ────────────────────── ──────────────────
2 sources               5 sources                All sources            Self-expanding
Basic classification    Automated clustering     Threshold alerts       PRD suggestions
Manual brief review     Scored themes            Trend analysis         Predictive alerts
Eval baseline           AMI Cycle 2              Executive dashboard    Multi-product
                                                 Roadmap integration
```

---

## Phase 1: Foundation (Weeks 1-4)
**Goal:** Prove the concept works. Get one PM trusting one brief.

### Deliverables
| Item | Details | Owner |
|------|---------|-------|
| Zendesk connector | Ingest support tickets via API | Engineering |
| App Store connector | Ingest iOS/Android reviews for Sailthru | Engineering |
| Classification pipeline | Bug / feature request / praise / complaint / question | Engineering + PM |
| Sentiment scoring | -1 to +1 with confidence | Engineering |
| Common schema | Normalize both sources to single format | Engineering |
| Manual weekly brief | PM reviews classifications, writes brief manually with AI assistance | PM |
| Eval baseline | 100-item human audit; establish accuracy baselines | PM |

### Success Criteria
- [ ] 95%+ ingestion completeness from both sources
- [ ] Classification accuracy >80% (human audit)
- [ ] Sentiment accuracy >85%
- [ ] PM rates manual brief process as "valuable" (qualitative)
- [ ] Processing latency <5 seconds per item

### Key Decisions
- Classification taxonomy finalized (5 categories + subcategories)
- Prompt templates locked for classification and sentiment
- Eval cadence established (100 items/week)

---

## Phase 2: Intelligence (Weeks 5-8)
**Goal:** Automate the intelligence layer. PM receives brief without manual assembly.

### Deliverables
| Item | Details | Owner |
|------|---------|-------|
| G2/Capterra connector | Public reviews for Marigold + top 3 competitors | Engineering |
| Survey connector | NPS/CSAT data from Typeform/Qualtrics | Engineering |
| Gong connector | Call transcript summaries | Engineering |
| Embedding generation | Vector embeddings for all processed feedback | Engineering |
| Theme clustering | HDBSCAN on embeddings; auto-labeled clusters | Engineering |
| Opportunity scoring | Weighted composite (frequency, sentiment, revenue, recency) | PM + Engineering |
| Automated weekly brief | LLM-generated intelligence brief, delivered Monday 7 AM | Engineering |
| AMI Cycle 2 | Expanded audit (1,000 items); prompt optimization | PM |

### Success Criteria
- [ ] 5 sources ingesting successfully
- [ ] Classification accuracy >85%
- [ ] Cluster coherence >80% (human agreement)
- [ ] Automated brief delivered on schedule 4/4 weeks
- [ ] Brief actionability rating >3.0/5.0 from PM
- [ ] At least 1 insight from brief influences a product decision

### Key Decisions
- Scoring weights finalized based on PM feedback
- Brief format locked (sections, length, delivery channel)
- Competitive data handling policy (separate from internal feedback)

---

## Phase 3: Proactive (Weeks 9-12)
**Goal:** System anticipates problems and opportunities before PM asks.

### Deliverables
| Item | Details | Owner |
|------|---------|-------|
| Community connector | Discourse/Slack community feedback | Engineering |
| Threshold alerting | Configurable per-theme spike detection | Engineering |
| Trend analysis | Week-over-week sentiment and volume trends | Engineering |
| Executive dashboard | High-level view for product leadership | Engineering |
| Roadmap tool integration | Push high-scoring themes to Linear/Productboard | Engineering |
| AMI Cycle 3 | Full pipeline audit; cost optimization | PM |

### Success Criteria
- [ ] All 6 sources ingesting
- [ ] Classification accuracy >90%
- [ ] Alert precision >80% (low false positives)
- [ ] PM reports 50%+ time savings on feedback review
- [ ] 2+ roadmap items directly influenced by CIE data
- [ ] Executive dashboard adopted by product leadership
- [ ] Cost per feedback item <$0.05

### Key Decisions
- Alert fatigue mitigation (frequency caps, severity tiers)
- Dashboard access controls (who sees what)
- Integration depth with roadmap tools (push vs. link)

---

## Phase 4: Autonomous (Weeks 13+)
**Goal:** System operates with minimal human oversight. Begins generating higher-order outputs.

### Deliverables
| Item | Details | Owner |
|------|---------|-------|
| PRD topic suggestions | Auto-generate PRD drafts from high-scoring theme clusters | PM + Engineering |
| Predictive alerting | Predict theme spikes before they happen using trend data | Engineering |
| Multi-product support | Separate intelligence streams for Sailthru, Cheetah, Selligent | Engineering |
| Self-improving classification | Auto-select few-shot examples from corrected items | Engineering |
| Churn signal detection | Cross-reference sentiment trends with retention data | PM + Data |
| Customer segment intelligence | Separate briefs per segment (enterprise, mid-market, SMB) | PM |

### Success Criteria
- [ ] PRD suggestions rated "relevant" >60% of the time
- [ ] Predictive alerts achieve 48-hour lead time on trend spikes
- [ ] Multi-product briefs adopted by all 3 product line PMs
- [ ] Classification accuracy maintained >90% with <50% of original human audit effort
- [ ] CIE data cited in 50%+ of quarterly roadmap reviews

---

## Dependencies & Risks by Phase

| Phase | Key Dependency | Risk | Mitigation |
|-------|---------------|------|------------|
| 1 | Zendesk API access | API rate limits on Marigold's plan | Negotiate API tier; implement respectful polling |
| 1 | PM availability for eval | PM too busy for weekly 100-item audit | Reduce to 50 items; extend Phase 1 by 1 week |
| 2 | Gong API access | May require enterprise Gong plan | Defer Gong to Phase 3; use manual transcript upload as interim |
| 2 | Clustering quality | HDBSCAN may produce poor clusters on small datasets | Supplement with LLM-based clustering as fallback |
| 3 | Roadmap tool API | Linear/Productboard integration complexity | Start with CSV export; automate integration in Phase 4 |
| 4 | Predictive model accuracy | Insufficient historical data for prediction | Require 12+ weeks of data before enabling; set clear confidence thresholds |

---

## Resource Requirements

| Role | Phase 1 | Phase 2 | Phase 3 | Phase 4 |
|------|---------|---------|---------|---------|
| PM (part-time) | 8 hrs/week | 6 hrs/week | 4 hrs/week | 2 hrs/week |
| Engineer (full-time) | 1 FTE | 1 FTE | 0.5 FTE | 0.25 FTE |
| Data Analyst | - | 4 hrs/week | 4 hrs/week | 2 hrs/week |
| API costs | ~$10/month | ~$25/month | ~$40/month | ~$60/month |

**Total estimated investment:** ~$80K over 16 weeks (1 FTE engineer + PM time + API costs)
**Expected ROI:** 200+ PM hours/month recovered across team of 10 PMs = ~$150K/year in reallocated capacity

---

_Roadmap follows phased approach from Gupta's "AI Prototype to Production" (2025): validate first, then scale._
