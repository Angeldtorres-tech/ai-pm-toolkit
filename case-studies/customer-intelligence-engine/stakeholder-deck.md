# Stakeholder Deck: AI Customer Intelligence Engine
## Executive Summary for Leadership Buy-In

**Author:** Angel Torres | **Date:** 2026-02-24
**Audience:** VP Product, CTO, Product Leadership Team
**Format:** 10-slide structure (present in 15 minutes)

---

## Slide 1: The Problem

**Title:** We're Deaf to 80% of Our Customers

- Marigold has 40,000+ customers across 3 product lines
- Feedback arrives through 6+ channels: support, app reviews, surveys, calls, community, G2
- Product teams manually review ~20% of total feedback
- Cross-channel patterns go undetected
- Insights arrive quarterly, but customers churn monthly

**Visual:** Funnel showing 10,000 monthly feedback items narrowing to 2,000 reviewed, with 8,000 labeled "Invisible to Product"

---

## Slide 2: The Cost of Not Listening

**Title:** What We're Missing (and What It Costs)

| Gap | Business Impact |
|-----|----------------|
| PMs spend 4-6 hrs/week manually reviewing feedback | ~$150K/year in PM time across team (that could go to strategy) |
| Churn signals detected after customer leaves | Lost ARR recovery window |
| Feature prioritization based on loudest voices | Misallocated engineering resources |
| No baseline for Zeta integration planning | Integration risks discovered too late |

**Key message:** This isn't a tooling problem. It's a visibility problem that compounds every quarter.

---

## Slide 3: The Solution

**Title:** AI Customer Intelligence Engine: Autonomous Feedback Intelligence

The CIE automatically:
1. **Ingests** feedback from all 6 channels (24/7, no manual effort)
2. **Analyzes** using LLM-powered classification, sentiment, and entity extraction
3. **Clusters** related feedback into scored themes using semantic similarity
4. **Delivers** a weekly intelligence brief with prioritized opportunities
5. **Alerts** when emerging issues cross configurable thresholds

**Key message:** PMs go from spending 5 hours reading tickets to 30 minutes reviewing actionable intelligence.

---

## Slide 4: How It Works (Architecture)

**Title:** Simple Architecture, Powerful Intelligence

**Visual:** Simplified data flow diagram

```
6 Sources → Ingestion → AI Processing → Theme Clustering → Weekly Brief to PMs
                              ↓                  ↓
                         Eval Engine         Alert Engine
                     (quality improves        (real-time
                       over time)             notifications)
```

- Uses Claude API for classification and synthesis ($40/month at scale)
- Built-in eval framework ensures accuracy improves with every cycle
- Modular: add new sources without rebuilding the pipeline

---

## Slide 5: What Makes This Different

**Title:** Why Not Just Use Productboard / Enterpret / ChatGPT?

| Approach | Limitation | CIE Advantage |
|----------|-----------|---------------|
| Productboard | Manual import; basic tagging; PM does the work | Autonomous; PM reads the output |
| Enterpret | Enterprise pricing ($50K+); black-box models | $500/year; transparent eval framework |
| Manual LLM (Claude/GPT) | One-time paste; no continuity; no correlation | Continuous; cross-channel; self-improving |
| Qualtrics | Survey-only; misses organic feedback | All channels; organic + structured |

**Key message:** No existing tool delivers autonomous, cross-channel, eval-driven customer intelligence at this cost.

---

## Slide 6: Zeta Integration Value

**Title:** Essential for the Zeta Transition

- Establish sentiment baseline across Sailthru, Cheetah, and Selligent before integration
- Detect integration-related customer concerns in real-time as changes roll out
- Compare customer satisfaction across product lines to inform consolidation decisions
- Give the combined product team a unified view of 40K+ customer voices

**Key message:** We're about to make major product changes. We need to hear customers at scale during the transition.

---

## Slide 7: Investment & Timeline

**Title:** 4 Weeks to MVP, $80K Total Over 16 Weeks

| Phase | Timeline | Investment | Outcome |
|-------|----------|-----------|---------|
| Foundation | Weeks 1-4 | 1 engineer + PM time | 2 sources, basic classification, manual brief |
| Intelligence | Weeks 5-8 | Same | 5 sources, auto clustering, automated brief |
| Proactive | Weeks 9-12 | 0.5 engineer | All sources, alerting, executive dashboard |
| Autonomous | Weeks 13+ | 0.25 engineer maintenance | Predictive alerts, PRD suggestions, multi-product |

**API cost:** ~$40/month at scale (Claude + embeddings)
**Total 16-week investment:** ~$80K (primarily engineering time)

---

## Slide 8: Expected Returns

**Title:** ROI: 2x Return in Year 1

| Metric | Current | With CIE | Value |
|--------|---------|----------|-------|
| PM time on feedback | 4-6 hrs/week each | <1.5 hrs/week | ~$150K/year saved (10 PMs) |
| Feedback coverage | 20% | 95%+ | Reduces blind spot risk |
| Time to detect issues | 2-4 weeks | <48 hours | Earlier churn intervention |
| Evidence-backed roadmap items | ~10% | 50%+ | Better prioritization ROI |

**Payback period:** ~6 months
**Year 1 ROI:** ~2x ($150K value on $80K investment)

---

## Slide 9: Risk & Governance

**Title:** Built Responsibly

- **Privacy:** All feedback anonymized before AI processing. No PII in API calls. GDPR compliant.
- **Accuracy:** Built-in eval framework (Analyze-Measure-Improve cycle). Classification accuracy target >90%.
- **Trust:** Every insight links to source feedback. PM validates; AI recommends.
- **Cost control:** Token budgets, batch processing, model right-sizing. No surprise bills.
- **Failure mode:** If the system goes down, PMs revert to manual process. No dependency risk.

**Key message:** This is AI done responsibly. Human-in-the-loop, transparent, and recoverable.

---

## Slide 10: The Ask

**Title:** What We Need to Start

1. **1 engineer** dedicated for 8 weeks (Phase 1-2)
2. **API budget** approval: $500/year (Claude + embeddings)
3. **Zendesk API access** for the product support instance
4. **1 PM** to serve as design partner and eval reviewer (4 hrs/week)
5. **Executive sponsor** to champion adoption across product lines

**We'll deliver:**
- Working MVP in 4 weeks
- Automated intelligence brief in 8 weeks
- Full proactive system in 12 weeks

**Next step:** Approve Phase 1 kickoff. First brief delivered in 30 days.

---

## Appendix: Questions We Expect

**Q: Why not wait for Zeta's tools?**
A: Zeta acquisition closes in months. We need customer intelligence during the transition, not after. CIE is lightweight and modular; it adapts to whatever platform emerges.

**Q: Can we trust AI classification?**
A: Not blindly. That's why we built an eval framework. We measure accuracy weekly, flag low-confidence items for human review, and improve prompts based on data. Trust is earned through transparency.

**Q: What if PMs ignore the briefs?**
A: We co-design the brief format with PMs during Phase 1. If adoption is low, we iterate on format before scaling. The brief must earn its place in the PM's Monday morning.

**Q: How does this scale to the combined Zeta entity?**
A: Modular architecture. Each data source is an independent connector. Adding Zeta's customer feedback channels is adding new connectors to an existing pipeline.

---

_Deck structure follows problem-solution-evidence-ask flow. Designed for 15-minute presentation with 5 minutes Q&A._
