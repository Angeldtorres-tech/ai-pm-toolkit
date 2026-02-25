# Product Requirements Document: AI Customer Intelligence Engine

**Author:** Angel Torres | **Version:** 1.0 | **Date:** 2026-02-24
**Framework:** Aakash Gupta's AI PM Stack (2025)
**Status:** Draft

---

## 1. Hypothesis

**If** we build an autonomous system that continuously ingests, clusters, and analyzes customer feedback from multiple channels,
**then** product teams will identify high-impact opportunity areas 3x faster than manual feedback review,
**because** LLM-powered semantic analysis can process thousands of data points and surface patterns that humans miss when reviewing feedback one ticket at a time.

---

## 2. Strategic Fit

### Problem Statement
Product teams drown in customer feedback. The average B2B SaaS company receives feedback through 5-8 channels (support tickets, NPS surveys, app reviews, sales calls, community forums, social media, feature request boards, customer success notes). Currently:

- **80% of feedback goes unread** by product teams (source: Productboard State of PM 2024)
- Feedback is siloed by channel, making cross-channel pattern detection nearly impossible
- PMs spend 4-6 hours/week manually reviewing and categorizing feedback
- Insights arrive too late: by the time a PM identifies a trend, the quarter is already planned

### Why AI is the Right Solution
Using Gupta's AI Product Decision Framework:
- ✅ High volume of unstructured data (thousands of data points monthly)
- ✅ Pattern recognition across diverse formats (text, ratings, transcripts)
- ✅ Speed matters (real-time trends vs quarterly review cycles)
- ✅ Human judgment still needed for strategic decisions (AI assists, doesn't replace)
- ✅ Clear eval criteria (clustering accuracy, insight relevance, action rate)

### Strategic Alignment
- **Level 2 capability (current):** LLMs can classify, cluster, and summarize text with high accuracy
- **Level 3 readiness:** Architecture designed to support autonomous agents that could draft PRD sections from insights

---

## 3. Product Overview

### What It Does
The AI Customer Intelligence Engine (CIE) is an autonomous system that:

1. **Ingests** customer feedback from multiple channels via API integrations
2. **Processes** feedback using LLM-powered semantic analysis (classification, sentiment, entity extraction)
3. **Clusters** related feedback into themes using embedding-based similarity
4. **Scores** themes by frequency, sentiment intensity, revenue impact, and recency
5. **Generates** weekly intelligence briefs with prioritized opportunity areas
6. **Alerts** when emerging themes cross configurable thresholds

### What It Does NOT Do (Non-Goals)
- Does not replace PM judgment on what to build
- Does not auto-generate PRDs or roadmap items
- Does not contact customers directly
- Does not make prioritization decisions (it informs them)
- Does not process audio/video directly (relies on transcription services)

---

## 4. Users & Personas

### Primary: Product Manager
- Reviews weekly intelligence briefs
- Uses theme data for roadmap planning
- Drills into specific clusters for context
- Configures alert thresholds

### Secondary: Product Leader (Director/VP)
- Reviews executive summary dashboards
- Uses trend data for strategic planning
- Monitors customer sentiment over time

### Tertiary: Customer Success Manager
- References intelligence briefs during customer calls
- Validates that customer concerns are being tracked
- Flags high-priority accounts with recurring issues

---

## 5. User Stories & Acceptance Criteria

### US-1: Automated Feedback Ingestion
**As a** PM, **I want** customer feedback from all channels to be automatically collected and normalized, **so that** I don't spend time manually aggregating data.

**Acceptance Criteria:**
- System connects to 3+ feedback channels (support tickets, app reviews, survey responses)
- New feedback is ingested within 15 minutes of creation
- All feedback is normalized to a common schema (text, source, timestamp, customer_id, sentiment)
- Duplicate detection prevents the same feedback from being counted multiple times
- Ingestion errors are logged and surface in a health dashboard

### US-2: Semantic Theme Clustering
**As a** PM, **I want** feedback automatically grouped into meaningful themes, **so that** I can see patterns without reading every individual piece of feedback.

**Acceptance Criteria:**
- System generates theme clusters using embedding similarity (cosine similarity > 0.75 threshold)
- Each cluster has an LLM-generated label and 2-sentence summary
- Clusters refresh daily with new feedback incorporated
- Orphan feedback (< 0.75 similarity to any cluster) is flagged for manual review
- Cluster quality eval: human reviewers agree with clustering 80%+ of the time

### US-3: Opportunity Scoring
**As a** PM, **I want** themes scored by business impact, **so that** I can prioritize which areas to investigate first.

**Acceptance Criteria:**
- Each theme receives a composite score based on: frequency (30%), sentiment intensity (25%), revenue impact (25%), recency (20%)
- Revenue impact estimated by cross-referencing customer_id with ARR data (where available)
- Scores update daily
- PM can adjust weight configuration
- Scoring rationale is transparent (show calculation breakdown)

### US-4: Weekly Intelligence Brief
**As a** PM, **I want** a weekly summary of customer intelligence, **so that** I have a regular cadence of insight without manual effort.

**Acceptance Criteria:**
- Brief generated automatically every Monday at 7 AM
- Includes: top 5 themes by score, emerging themes (new this week), sentiment trends, notable individual feedback
- Brief is delivered via email and available in dashboard
- Brief is 1 page max (concise, scannable)
- Each theme links to underlying feedback for drill-down

### US-5: Threshold Alerts
**As a** PM, **I want** to be notified when a theme spikes, **so that** I can respond to emerging issues quickly.

**Acceptance Criteria:**
- PM can set thresholds per theme (e.g., "alert me if > 20 mentions in 48 hours")
- Alerts delivered via Slack/email
- Alert includes: theme summary, spike magnitude, sample feedback, suggested action
- Alert deduplication (don't alert on the same spike twice)

---

## 6. Technical Architecture

### Data Flow
```
[Support Tickets] ──┐
[App Reviews]    ──┤
[NPS Surveys]    ──┼──> Ingestion Layer ──> Processing Pipeline ──> Theme Store ──> Intelligence Brief
[Community Posts] ──┤                         │                       │
[Sales Call Notes]──┘                    [LLM Analysis]         [Eval Engine]
                                         - Classify                - AMI Cycle
                                         - Extract entities        - Quality scoring
                                         - Sentiment               - Drift detection
                                         - Generate embeddings
```

### Technology Choices
- **Ingestion:** API connectors (Zendesk, Intercom, App Store Connect, Typeform, Discourse)
- **Processing:** Claude API for classification/summarization, embedding model for clustering
- **Storage:** PostgreSQL (structured data) + vector store (embeddings) 
- **Orchestration:** n8n or Temporal for pipeline scheduling
- **Delivery:** Email (SendGrid), Slack (webhook), web dashboard
- **Eval:** Custom eval framework following Gupta/Husain AMI cycle

### AI Pipeline Detail
1. **Ingestion normalization:** Raw feedback mapped to common schema
2. **Classification (Claude):** Categorize by type (bug, feature request, praise, complaint, question)
3. **Entity extraction (Claude):** Extract product areas, features mentioned, customer segment
4. **Sentiment analysis (Claude):** Score sentiment (-1 to +1) with confidence
5. **Embedding generation:** Generate vector embeddings for similarity clustering
6. **Clustering:** HDBSCAN on embeddings, with LLM-generated cluster labels
7. **Scoring:** Composite score calculation with configurable weights
8. **Brief generation (Claude):** Summarize top themes into scannable intelligence brief

---

## 7. Eval Framework (Gupta/Husain AMI Cycle)

### Analyze Phase
- Review 100 random feedback items manually
- Compare system classification vs human classification
- Identify failure modes: misclassification, missed entities, incorrect sentiment

### Measure Phase
| Metric | Target | Method |
|--------|--------|--------|
| Classification accuracy | > 85% | Human review of 100-item sample weekly |
| Clustering coherence | > 80% agreement | Human eval of cluster membership |
| Sentiment accuracy | > 90% | Compare to human-labeled sentiment |
| Brief actionability | > 70% "useful" rating | PM survey after each brief |
| Alert precision | > 80% | Track false positive rate |
| Ingestion completeness | > 95% | Compare source count vs ingested count |

### Improve Phase
- Misclassifications: refine prompts with few-shot examples from failures
- Clustering drift: adjust similarity threshold based on coherence scores
- Brief quality: iterate on brief prompt using PM feedback (drafts 4-5 rule)

---

## 8. Guardrails & Governance

### Data Governance
- PII handling: customer names and emails are stripped from analysis; only anonymized feedback text is processed
- Data retention: raw feedback retained for 12 months, embeddings for 6 months
- Access control: only PM team and CS leadership can access intelligence briefs
- No customer data sent to external APIs without anonymization

### AI Guardrails
- Confidence threshold: classifications below 0.7 confidence are flagged for human review
- Hallucination prevention: briefs reference specific feedback IDs; no "invented" statistics
- Human-in-the-loop: all strategic recommendations are suggestions, never auto-executed
- Fallback: if LLM API is unavailable, system queues feedback and alerts PM of delay

### Risk Register
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| LLM misclassifies critical feedback | Medium | High | Confidence threshold + human review queue |
| Clustering creates meaningless groups | Medium | Medium | Weekly coherence eval + threshold tuning |
| Over-reliance on automated insights | Low | High | Brief explicitly states "AI-assisted, not AI-decided" |
| Data breach via API integration | Low | Critical | OAuth2, encrypted transit, no PII in analysis |
| Cost overrun from LLM API calls | Medium | Medium | Batch processing, caching, token budgets |

---

## 9. Rollout Strategy

### Phase 1: MVP (Weeks 1-4)
- 2 feedback channels connected (support tickets + app reviews)
- Basic classification and sentiment
- Manual clustering review
- Simple weekly email brief
- **Success criteria:** PM finds brief "useful" 3/4 weeks

### Phase 2: Intelligence (Weeks 5-8)
- All 5 channels connected
- Automated clustering with quality scoring
- Opportunity scoring with configurable weights
- Dashboard with drill-down
- **Success criteria:** Classification accuracy > 85%, clustering coherence > 80%

### Phase 3: Proactive (Weeks 9-12)
- Threshold alerts
- Trend analysis (week-over-week sentiment shifts)
- Executive summary dashboard
- Integration with roadmap tools (Linear, Productboard)
- **Success criteria:** PM reports 50%+ time savings in feedback review

### Phase 4: Autonomous (Weeks 13+)
- System suggests PRD topics based on theme clusters
- Predictive alerting (predict spikes before they happen based on trends)
- Multi-product support
- **Success criteria:** Insights directly influence 2+ roadmap items per quarter

---

## 10. Success Metrics

| Metric | Baseline | Target | Timeframe |
|--------|----------|--------|-----------|
| PM time spent on feedback review | 4-6 hrs/week | < 1.5 hrs/week | 12 weeks |
| Feedback coverage (% read/analyzed) | ~20% | > 95% | 8 weeks |
| Time to identify emerging trend | 2-4 weeks | < 48 hours | 8 weeks |
| Roadmap items influenced by CIE data | 0 | 2+ per quarter | 16 weeks |
| PM satisfaction (brief usefulness) | N/A | > 70% "useful" | 4 weeks |

---

## 11. What This Demonstrates (Portfolio Value)

| Skill | How This PRD Shows It |
|-------|-----------------------|
| AI Product Sense | Chose AI because the problem has clear AI-fit criteria (volume, unstructured data, pattern recognition) |
| Customer Empathy | Multiple personas, real user stories with measurable acceptance criteria |
| System Design | Full architecture with data flows, technology choices, pipeline detail |
| AI Evals | AMI cycle applied with specific metrics and improvement strategies |
| Governance | Data privacy, guardrails, risk register, human-in-the-loop design |
| Strategic Thinking | Phased rollout from MVP to autonomous, clear success criteria per phase |
| Prioritization | Opportunity scoring with transparent, configurable methodology |
| Communication | Concise brief format, stakeholder-appropriate detail levels |

---

_PRD follows Aakash Gupta's "AI PRDs in 2025" template (co-authored with Miqdad Jaffer, OpenAI)._
_Eval framework follows Gupta/Husain's AMI cycle from "AI Evals: Everything You Need to Know."_
