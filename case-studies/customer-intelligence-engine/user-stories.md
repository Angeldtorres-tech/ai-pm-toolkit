# User Stories & Acceptance Criteria: AI Customer Intelligence Engine

**Author:** Angel Torres | **Date:** 2026-02-24

---

## Personas Reference

| Persona | Role | Key Need | Frequency of Use |
|---------|------|----------|-----------------|
| Maya | Product Manager, Sailthru | Understand what customers want; prioritize roadmap | Daily (alerts) + Weekly (brief) |
| David | VP of Product, Marigold | Strategic view across all product lines; board reporting | Weekly (brief) + Monthly (dashboard) |
| Sarah | Customer Success Manager | Know what customers are complaining about before calls | Daily (search/alerts) |

---

## Epic 1: Feedback Ingestion

### US-1.1: Connect Support Ticket Source
**As** Maya (PM), **I want** support tickets from Zendesk automatically flowing into the system, **so that** I don't have to manually export and review tickets each week.

**Acceptance Criteria:**
- Given Zendesk API credentials are configured, when a new ticket is created/updated, then it appears in the system within 15 minutes
- Given a ticket contains customer PII, when it is ingested, then PII is stripped before processing
- Given the Zendesk API is temporarily unavailable, when connectivity is restored, then missed tickets are backfilled
- Given a duplicate ticket (same ID), when ingested again, then it is not counted twice

**Story Points:** 5 | **Priority:** P0

### US-1.2: Connect App Store Reviews
**As** Maya (PM), **I want** iOS and Android app reviews automatically ingested, **so that** I can see what public users are saying without manually checking app stores.

**Acceptance Criteria:**
- Given App Store Connect and Google Play credentials, when a new review is posted, then it appears within 1 hour
- Given a review has a star rating, when ingested, then the rating is preserved in metadata
- Given a review is in a non-English language, when detected, then it is flagged for translation pipeline

**Story Points:** 5 | **Priority:** P0

### US-1.3: Connect Public Review Sites
**As** Maya (PM), **I want** G2 and Capterra reviews for our products and competitors automatically monitored, **so that** I can track competitive sentiment without manual research.

**Acceptance Criteria:**
- Given configured product URLs on G2/Capterra, when new reviews are published, then they are ingested within 24 hours
- Given a review is for a competitor product, when ingested, then it is tagged with competitor name
- Given reviews have structured pros/cons, when ingested, then pros and cons are parsed separately

**Story Points:** 8 | **Priority:** P1

### US-1.4: Connect Survey Responses
**As** Maya (PM), **I want** NPS and CSAT survey responses automatically flowing in, **so that** quantitative satisfaction data enriches the qualitative feedback.

**Acceptance Criteria:**
- Given Typeform/Qualtrics webhook, when a survey response is submitted, then it appears within 5 minutes
- Given a response includes both a numeric score and free-text, when ingested, then both are preserved
- Given a response has no free-text comment, when ingested, then only the numeric score is stored (no LLM processing needed)

**Story Points:** 3 | **Priority:** P1

### US-1.5: Connect Call Transcripts
**As** Maya (PM), **I want** sales and CS call transcripts automatically analyzed, **so that** I hear what customers say in conversations, not just tickets.

**Acceptance Criteria:**
- Given Gong API access, when a call is completed and transcribed, then the transcript is ingested within 2 hours
- Given a transcript is long (>5000 words), when ingested, then it is chunked into logical segments for processing
- Given multiple speakers in a transcript, when processed, then only customer segments are analyzed (not internal rep speech)

**Story Points:** 8 | **Priority:** P2

### US-1.6: Ingestion Health Dashboard
**As** Maya (PM), **I want** to see the health of all data sources at a glance, **so that** I know if any source has silently failed.

**Acceptance Criteria:**
- Given 5 configured sources, when I open the health dashboard, then I see: source name, last successful ingestion time, items ingested today, status (healthy/warning/error)
- Given a source hasn't ingested in >24 hours, when displayed, then it shows "warning" status
- Given a source has thrown 5+ consecutive errors, when displayed, then it shows "error" status and I receive an alert

**Story Points:** 3 | **Priority:** P1

---

## Epic 2: AI Processing

### US-2.1: Automatic Classification
**As** Maya (PM), **I want** every piece of feedback automatically categorized, **so that** I can filter and analyze by type.

**Acceptance Criteria:**
- Given new feedback enters the queue, when processed, then it receives one of: bug_report, feature_request, praise, complaint, question, other
- Given the classification confidence is below 0.7, when classified, then it is routed to human review queue
- Given the classification prompt is updated, when reprocessed, then historical items can be reclassified in batch

**Story Points:** 5 | **Priority:** P0

### US-2.2: Sentiment Scoring
**As** Maya (PM), **I want** sentiment scored for every piece of feedback, **so that** I can track emotional trends over time.

**Acceptance Criteria:**
- Given feedback text, when processed, then it receives a sentiment score from -1.0 (very negative) to +1.0 (very positive) with confidence score
- Given mixed-sentiment feedback ("love the product but the API is broken"), when scored, then it receives a nuanced score (e.g., 0.1) rather than forced positive/negative
- Given sentiment confidence below 0.6, when scored, then it is excluded from aggregate sentiment calculations

**Story Points:** 3 | **Priority:** P0

### US-2.3: Entity Extraction
**As** Maya (PM), **I want** product areas and features automatically extracted from feedback, **so that** I can see which parts of the product are generating the most discussion.

**Acceptance Criteria:**
- Given feedback text and product taxonomy, when processed, then extracted entities match entries in the taxonomy
- Given feedback mentions a feature by nickname or abbreviation, when processed, then it maps to the canonical feature name
- Given no product entity is identifiable, when processed, then it is tagged as "general"

**Story Points:** 5 | **Priority:** P1

### US-2.4: Embedding Generation
**As the system**, feedback needs vector embeddings generated **so that** semantic clustering can group similar feedback regardless of exact wording.

**Acceptance Criteria:**
- Given processed feedback, when embedding is generated, then a 1024-dimension vector is stored in the vector database
- Given embedding generation fails, when error occurs, then the item is queued for retry (max 3 attempts)
- Given batch processing, when 100+ items are queued, then embeddings are generated in batch for efficiency

**Story Points:** 3 | **Priority:** P1

---

## Epic 3: Intelligence

### US-3.1: Theme Clustering
**As** Maya (PM), **I want** related feedback automatically grouped into themes, **so that** I can see patterns without reading every item.

**Acceptance Criteria:**
- Given 500+ processed items with embeddings, when clustering runs, then themes are generated with: auto-label, 2-sentence summary, item count, average sentiment
- Given a new theme emerges (didn't exist last week), when detected, then it is flagged as "emerging"
- Given an item doesn't fit any cluster (cosine similarity <0.75 to all), when processed, then it is placed in "unclustered" for manual review
- Given cluster coherence drops below 70%, when detected, then system alerts for parameter tuning

**Story Points:** 8 | **Priority:** P0

### US-3.2: Opportunity Scoring
**As** Maya (PM), **I want** themes scored by business impact, **so that** I know where to focus first.

**Acceptance Criteria:**
- Given a theme cluster, when scored, then it receives a composite score: frequency (30%) + sentiment intensity (25%) + revenue impact (25%) + recency (20%)
- Given I want different weights, when I adjust the configuration, then scores recalculate within 1 hour
- Given a theme's score changes significantly (>20%) week-over-week, when detected, then it is flagged in the brief

**Story Points:** 5 | **Priority:** P0

### US-3.3: Weekly Intelligence Brief
**As** Maya (PM), **I want** a weekly summary delivered to my inbox every Monday at 7 AM, **so that** I start my week with customer intelligence.

**Acceptance Criteria:**
- Given it is Monday 7 AM ET, when the brief generates, then it includes: top 5 themes by score, emerging themes (new this week), sentiment trend (overall + by product), 3 notable individual feedback items, action suggestions
- Given the brief, when I read it, then every data point links to underlying feedback for drill-down
- Given the brief, when rendered, then it fits on 1 printed page (concise, scannable)
- Given brief generation fails, when error occurs, then I receive a notification and brief is delivered within 2 hours

**Story Points:** 8 | **Priority:** P0

### US-3.4: Threshold Alerts
**As** Maya (PM), **I want** to be notified immediately when a theme spikes, **so that** I can respond to emerging issues in real-time.

**Acceptance Criteria:**
- Given I set a threshold (e.g., >20 mentions in 48 hours), when the threshold is crossed, then I receive a Slack/email alert within 15 minutes
- Given an alert fires, when delivered, then it includes: theme summary, spike magnitude, representative feedback samples, suggested next steps
- Given the same theme triggered an alert <24 hours ago, when threshold crossed again, then alert is suppressed (deduplication)

**Story Points:** 5 | **Priority:** P1

---

## Epic 4: Executive View

### US-4.1: Product Leader Dashboard
**As** David (VP Product), **I want** a high-level dashboard showing customer sentiment across all product lines, **so that** I can report to the board and make strategic decisions.

**Acceptance Criteria:**
- Given dashboard access, when I open it, then I see: overall sentiment trend, top themes across all products, product-by-product comparison, quarter-over-quarter trends
- Given I click on a theme, when drilled down, then I see underlying feedback items and source distribution
- Given I need board reporting, when I export, then data exports as CSV or formatted PDF

**Story Points:** 8 | **Priority:** P2

---

## Story Map Summary

| Priority | Epic | Stories | Total Points |
|----------|------|---------|-------------|
| P0 | Ingestion (core) | US-1.1, US-1.2 | 10 |
| P0 | Processing (core) | US-2.1, US-2.2 | 8 |
| P0 | Intelligence (core) | US-3.1, US-3.2, US-3.3 | 21 |
| P1 | Ingestion (expand) | US-1.3, US-1.4, US-1.6 | 14 |
| P1 | Processing (expand) | US-2.3, US-2.4 | 8 |
| P1 | Intelligence (alerts) | US-3.4 | 5 |
| P2 | Ingestion (deep) | US-1.5 | 8 |
| P2 | Executive | US-4.1 | 8 |

**Total:** 82 story points across 15 stories
**MVP (P0 only):** 39 story points across 7 stories = ~4 weeks with 1 engineer

---

_User stories follow standard Agile format with Given/When/Then acceptance criteria. Story points use modified Fibonacci sequence._
