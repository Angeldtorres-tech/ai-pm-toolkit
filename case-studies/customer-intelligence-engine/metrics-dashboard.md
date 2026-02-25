# Metrics Dashboard Specification: AI Customer Intelligence Engine

**Author:** Angel Torres | **Date:** 2026-02-24

---

## 1. Dashboard Overview

Three dashboard views for three audiences:

| View | Audience | Purpose | Refresh Rate |
|------|----------|---------|-------------|
| PM Intelligence | Product Managers | Daily insight consumption + drill-down | Real-time |
| System Health | PM + Engineering | Pipeline monitoring + eval metrics | Real-time |
| Executive Summary | VP Product + Leadership | Strategic trends + cross-product view | Weekly |

---

## 2. PM Intelligence Dashboard

### Section A: Weekly Brief Summary
- Current week's top 5 themes with scores
- Emerging themes (new this week) highlighted
- Overall sentiment trend (4-week sparkline)
- Link to full brief PDF/email

### Section B: Theme Explorer
- Interactive list of all active themes
- Sortable by: score, frequency, sentiment, recency
- Click to expand: see underlying feedback items, source distribution, sentiment over time
- Filter by: product line, source, classification, date range, customer segment

### Section C: Alert Feed
- Chronological list of triggered alerts
- Each alert shows: theme, trigger condition, magnitude, timestamp
- Status: acknowledged / investigating / resolved
- Click to see alert detail and recommended actions

### Section D: Feedback Search
- Full-text search across all ingested feedback
- Filter by: classification, sentiment range, source, product, date
- Results show: text snippet, classification, sentiment, source, date

---

## 3. System Health Dashboard

### Section A: Ingestion Status
| Metric | Display | Alert Threshold |
|--------|---------|----------------|
| Items ingested (today) | Counter per source | <50% of daily average |
| Last successful ingestion | Timestamp per source | >24 hours ago |
| Ingestion errors (today) | Counter with error log link | >5 errors |
| Deduplication rate | Percentage | >20% (indicates source issue) |

### Section B: Processing Pipeline
| Metric | Display | Alert Threshold |
|--------|---------|----------------|
| Items in queue | Counter | >500 (backlog) |
| Processing latency (P50/P95) | Line chart | P95 >5 seconds |
| Classification distribution | Pie chart (today vs 7-day avg) | >15% shift in any category |
| Low-confidence items | Counter + percentage | >20% of total |

### Section C: Eval Metrics
| Metric | Display | Alert Threshold |
|--------|---------|----------------|
| Classification accuracy | Gauge (0-100%) | <80% |
| Sentiment accuracy | Gauge (0-100%) | <85% |
| Cluster coherence | Gauge (0-100%) | <75% |
| Brief actionability | Gauge (1-5 scale) | <3.0 |
| Human review queue size | Counter | >50 items |

### Section D: Cost Tracking
| Metric | Display | Alert Threshold |
|--------|---------|----------------|
| API spend (today) | Dollar counter | >$5/day |
| API spend (month-to-date) | Dollar counter + trend | >$100/month |
| Tokens consumed (today) | Counter by model | >500K tokens/day |
| Cost per feedback item | Dollar average | >$0.10 |

---

## 4. Executive Summary Dashboard

### Section A: Sentiment Overview
- Overall customer sentiment score (3-month trend line)
- Sentiment by product line (Sailthru, Cheetah, Selligent) comparison
- Sentiment by customer segment (Enterprise, Mid-Market, SMB)
- Quarter-over-quarter sentiment change with delta indicators

### Section B: Top Themes (Cross-Product)
- Top 10 themes across all products, ranked by opportunity score
- Visual indicator: rising/stable/declining trend
- Click to see product-specific breakdown

### Section C: Key Metrics
- Total feedback analyzed this month (vs. previous month)
- Coverage rate (% of total feedback processed)
- Roadmap items influenced by CIE data this quarter
- PM time savings estimate

### Section D: Competitive Pulse
- Competitor sentiment comparison (from G2/Capterra data)
- New competitor themes (features they're being praised/criticized for)
- Market positioning indicator

---

## 5. KPI Tracking Plan

### Leading Indicators (predict future success)
| KPI | Target | Measurement | Cadence |
|-----|--------|-------------|---------|
| PM dashboard login rate | >3x/week per PM | Analytics tracking | Weekly |
| Brief open rate | >80% | Email tracking | Weekly |
| Alert acknowledgment rate | >90% within 4 hours | System tracking | Daily |
| Human review completion rate | >95% within 48 hours | System tracking | Weekly |

### Lagging Indicators (confirm value delivery)
| KPI | Target | Measurement | Cadence |
|-----|--------|-------------|---------|
| Roadmap items influenced by CIE | 2+/quarter | PM self-report | Quarterly |
| PM time on feedback review | <1.5 hrs/week | PM survey | Monthly |
| Theme-to-action conversion rate | >30% | Track themes that become tickets/PRDs | Quarterly |
| Customer churn predicted before occurrence | >50% of churn cases | Retrospective analysis | Quarterly |

### System Health Indicators
| KPI | Target | Measurement | Cadence |
|-----|--------|-------------|---------|
| Uptime | >99.5% | System monitoring | Daily |
| Classification accuracy | >90% | Human audit | Weekly |
| Brief delivery on-time rate | 100% | System tracking | Weekly |
| API cost per item | <$0.05 | Billing data | Monthly |

---

## 6. Reporting Cadence

| Report | Audience | Frequency | Format |
|--------|----------|-----------|--------|
| Weekly Intelligence Brief | PMs + CS | Monday 7 AM | Email + dashboard |
| System Health Summary | PM + Engineering | Daily (automated) | Slack digest |
| Eval Accuracy Report | PM team | Weekly | Dashboard + Slack |
| Executive Summary | VP Product | Monthly | Dashboard + PDF export |
| Quarterly Business Impact | Leadership | Quarterly | Presentation deck |

---

_Dashboard specs inform engineering implementation. Prioritize PM Intelligence view for Phase 1, add System Health in Phase 2, Executive Summary in Phase 3._
