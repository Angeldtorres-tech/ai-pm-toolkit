# Product Strategy Brief: AI Customer Intelligence Engine
## Contextualized for Marigold (Marketing Technology)

**Author:** Angel Torres | **Date:** 2026-02-24
**Framework:** Aakash Gupta's "AI Product Strategy" (2025, co-authored with Miqdad Jaffer, OpenAI)

---

## 1. Strategic Context

### Company Background
Marigold is a marketing technology company serving 40,000+ customers across loyalty, email marketing, and personalization. Their AI brand "MAI" operates across four pillars. They are being acquired by Zeta Global (announced Sept 2025), creating urgency to demonstrate AI-native capabilities.

### The Problem We're Solving
Marigold's product team receives customer feedback through 6+ channels:
- Support tickets (Zendesk/Intercom) from 40K+ customers
- App store reviews for Sailthru, Cheetah Digital, and Selligent products
- NPS and CSAT surveys post-onboarding and post-support
- G2 and Capterra reviews (public, competitive signal)
- Sales call transcripts from Gong/Chorus
- Community forum posts and feature request boards

**Current state:** Product teams manually review a fraction of this feedback. Insights arrive quarterly at best. Cross-channel patterns (e.g., Sailthru customers complaining about the same integration issue that Cheetah Digital customers praise) go undetected.

**Business impact of the gap:**
- Feature prioritization based on loudest voices, not largest patterns
- Churn signals detected too late to intervene
- Competitive positioning misses real-time market shifts
- Zeta integration planning lacks systematic customer sentiment baseline

### AI Product Decision Framework Check
Following Gupta's framework, we validate AI is the right approach:

| Criterion | Assessment | Score |
|-----------|-----------|-------|
| Volume of data | 40K+ customers, thousands of monthly feedback points | ✅ High |
| Data is unstructured | Text from 6+ channels in different formats | ✅ High |
| Pattern recognition needed | Cross-channel, cross-product correlation | ✅ High |
| Speed matters | Real-time vs quarterly review cycles | ✅ High |
| Human judgment still needed | PMs decide what to build; AI surfaces what to consider | ✅ Appropriate |
| Clear success criteria | Measurable accuracy, coverage, time savings | ✅ Defined |

**Verdict:** Strong AI fit. This is not "sprinkling AI on it." This is solving a fundamental data-processing bottleneck that humans cannot solve at scale.

---

## 2. Strategic Principles Applied

### Principle 1: Problem-First, Not Tech-First
We start with the PM's problem: "I don't know what my customers collectively want because I can't read 10,000 pieces of feedback per month."

We do NOT start with: "LLMs can classify text, so let's classify feedback."

The technology choice (LLM-powered semantic analysis) follows from the problem, not the other way around.

### Principle 2: Build for Level 2, Prep for Level 3
**Level 2 (current):** LLMs classify, cluster, and summarize feedback with high accuracy. The system requires human review of weekly briefs and human decision-making on priorities.

**Level 3 (future-ready):** Architecture supports autonomous agents that could:
- Auto-generate PRD drafts from high-scoring theme clusters
- Trigger customer outreach when churn signals cross thresholds
- Self-adjust classification taxonomy as products evolve

We build Level 2 now. The modular architecture (ingestion separate from processing separate from delivery) means Level 3 capabilities plug in without rebuilding.

### Principle 3: Invisible Intelligence
The PM doesn't interact with an "AI feedback tool." They receive a weekly intelligence brief in their inbox that feels like a senior analyst wrote it. The AI is invisible. The insight is visible.

No "AI" badge. No special interface to learn. Just better information, delivered on cadence.

---

## 3. Competitive Landscape

### Existing Solutions
| Solution | What It Does | Gap |
|----------|-------------|-----|
| Productboard | Centralized feedback portal with basic AI tagging | Requires manual data import; limited cross-channel correlation; no autonomous briefing |
| Dovetail | User research repository with AI analysis | Research-focused, not continuous monitoring; no real-time alerting |
| Qualtrics | Survey-based feedback with statistical analysis | Survey-only; misses organic feedback channels; enterprise-heavy setup |
| MonkeyLearn | Text classification API | Raw API, no product layer; requires engineering to operationalize |
| Native LLM (ChatGPT/Claude) | One-time paste-and-analyze | No continuity; no cross-channel correlation; no autonomous monitoring; manual every time |

### Our Differentiation
1. **Autonomous and continuous:** Runs 24/7, not when a PM remembers to check
2. **Cross-channel correlation:** Connects patterns across support, reviews, sales, and community
3. **Opinionated output:** Doesn't give raw data; delivers scored, prioritized, actionable briefs
4. **Eval-driven quality:** Built-in AMI cycle ensures accuracy improves over time
5. **PM-native delivery:** Brief format designed for PM workflows, not data analyst workflows

---

## 4. Value Proposition

### For the PM (Primary User)
"Stop spending 5 hours/week reading tickets. Start spending 30 minutes reviewing an intelligence brief that covers 95% of your feedback."

### For the Product Leader (Secondary User)
"Make roadmap decisions backed by quantitative customer signal, not anecdotal evidence from the last QBR."

### For the Business (Marigold-specific)
"As we integrate with Zeta, establish a data-driven baseline of customer sentiment across all three product lines. Know exactly where customers are happy, where they're at risk, and where the integration creates opportunity."

---

## 5. Success Metrics (Tied to Business Outcomes)

| Metric | Current State | Target | Business Impact |
|--------|-------------|--------|-----------------|
| Feedback coverage | ~20% manually reviewed | 95%+ auto-analyzed | No blind spots in customer sentiment |
| Time to insight | 2-4 weeks (quarterly reviews) | < 48 hours | React to emerging issues before churn |
| PM time on feedback review | 4-6 hrs/week | < 1.5 hrs/week | Reallocated to strategic work |
| Roadmap items influenced by data | ~10% evidence-backed | 50%+ evidence-backed | Better prioritization, fewer misses |
| Churn signal detection | Reactive (after churn) | Proactive (7-day lead) | Retention intervention window |

---

## 6. Strategic Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| PM ignores briefs (adoption failure) | Medium | High | Co-design brief format with PMs; iterate on relevance; track open/action rates |
| Classification accuracy too low for trust | Medium | High | Launch with human-reviewed MVP; build trust through AMI cycle; publish accuracy metrics |
| Zeta integration disrupts data sources | Medium | Medium | Modular ingestion layer; each source is independently swappable |
| Over-reliance on AI insights | Low | High | Every brief includes "AI-assisted, human-decided" framing; show confidence scores |
| Cost escalation from LLM API calls | Medium | Medium | Batch processing; use smaller models for classification; reserve large models for synthesis |

---

## 7. Go/No-Go Recommendation

**Recommendation: GO**

**Rationale:**
- Problem is real, measurable, and validated by PM workflow research
- AI fit is strong across all six criteria
- Competitive gap is clear (no existing tool does autonomous cross-channel intelligence)
- MVP can be built in 4 weeks and validated with a single PM
- Architecture supports incremental complexity without rebuild
- Directly supports Marigold's Zeta integration planning

**Investment required:** 1 PM (part-time), 1 engineer (full-time), Claude API budget (~$200/month at scale)

**Expected ROI:** 4-6 hours/week per PM recovered. With 10 PMs, that's 200+ hours/month of strategic capacity unlocked.

---

_Strategy follows Aakash Gupta's "Your Guide to AI Product Strategy" (2025), applying the AI Product Decision Framework and three non-negotiable principles (Problem-First, Level 2/3 Readiness, Invisible Intelligence)._
