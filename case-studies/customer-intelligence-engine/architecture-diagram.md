# Technical Architecture: AI Customer Intelligence Engine

**Author:** Angel Torres | **Date:** 2026-02-24

---

## System Architecture Diagram (Mermaid)

```mermaid
graph TB
    subgraph Sources["Data Sources"]
        ZD[Zendesk/Intercom<br/>Support Tickets]
        AS[App Store Connect<br/>Reviews]
        G2[G2/Capterra<br/>Public Reviews]
        SV[Typeform/Qualtrics<br/>Surveys]
        GG[Gong/Chorus<br/>Call Transcripts]
        CM[Discourse/Slack<br/>Community]
    end

    subgraph Ingestion["Ingestion Layer"]
        API[API Connectors<br/>OAuth2 + Webhooks]
        NORM[Normalizer<br/>Common Schema]
        DEDUP[Deduplication<br/>Engine]
        QUEUE[Message Queue<br/>Redis/SQS]
    end

    subgraph Processing["AI Processing Pipeline"]
        CLASS[Classifier<br/>Claude API]
        SENT[Sentiment Analyzer<br/>Claude API]
        ENT[Entity Extractor<br/>Claude API]
        EMB[Embedding Generator<br/>Voyage/OpenAI]
    end

    subgraph Intelligence["Intelligence Layer"]
        CLUST[Theme Clustering<br/>HDBSCAN]
        SCORE[Opportunity Scorer<br/>Weighted Composite]
        TREND[Trend Detector<br/>Time-Series Analysis]
        ALERT[Alert Engine<br/>Threshold Monitor]
    end

    subgraph Storage["Data Layer"]
        PG[(PostgreSQL<br/>Structured Data)]
        VS[(Vector Store<br/>Embeddings)]
        CACHE[(Redis<br/>Cache + Queue)]
    end

    subgraph Delivery["Delivery Layer"]
        BRIEF[Brief Generator<br/>Claude API]
        EMAIL[Email Delivery<br/>SendGrid]
        SLACK[Slack Webhook]
        DASH[Web Dashboard<br/>Next.js]
    end

    subgraph Eval["Eval & Governance"]
        AMI[AMI Cycle Engine<br/>Analyze-Measure-Improve]
        QUAL[Quality Scorer]
        DRIFT[Drift Detector]
        LOG[Audit Logger]
    end

    Sources --> API
    API --> NORM
    NORM --> DEDUP
    DEDUP --> QUEUE
    QUEUE --> CLASS
    QUEUE --> SENT
    QUEUE --> ENT
    QUEUE --> EMB
    CLASS --> PG
    SENT --> PG
    ENT --> PG
    EMB --> VS
    PG --> CLUST
    VS --> CLUST
    CLUST --> SCORE
    SCORE --> TREND
    TREND --> ALERT
    SCORE --> BRIEF
    BRIEF --> EMAIL
    BRIEF --> SLACK
    BRIEF --> DASH
    ALERT --> EMAIL
    ALERT --> SLACK
    PG --> AMI
    AMI --> QUAL
    AMI --> DRIFT
    AMI --> LOG
    QUAL --> CLASS
    DRIFT --> CLUST
```

---

## Data Flow Sequence

```mermaid
sequenceDiagram
    participant S as Data Source
    participant I as Ingestion Layer
    participant P as Processing Pipeline
    participant D as Data Store
    participant IL as Intelligence Layer
    participant DL as Delivery Layer
    participant PM as Product Manager

    Note over S,PM: Continuous Ingestion (Real-time / Polling)
    S->>I: New feedback arrives
    I->>I: Normalize to common schema
    I->>I: Deduplicate
    I->>P: Queue for processing

    Note over P,D: AI Processing (Batch every 15 min)
    P->>P: Classify (bug/feature/praise/complaint)
    P->>P: Extract entities (product, feature, segment)
    P->>P: Score sentiment (-1 to +1)
    P->>P: Generate embedding vector
    P->>D: Store structured data + embeddings

    Note over D,IL: Intelligence (Daily refresh)
    D->>IL: Fetch processed feedback
    IL->>IL: Cluster themes (HDBSCAN on embeddings)
    IL->>IL: Score opportunities (frequency + sentiment + revenue + recency)
    IL->>IL: Detect trends (week-over-week changes)
    IL->>IL: Check alert thresholds

    Note over IL,PM: Delivery (Weekly brief + real-time alerts)
    IL->>DL: Generate intelligence brief (top themes, emerging signals)
    DL->>PM: Email weekly brief (Monday 7 AM)
    IL->>DL: Threshold exceeded alert
    DL->>PM: Slack/email alert (real-time)
```

---

## Component Detail

### Ingestion Layer
| Component | Technology | Purpose | Scaling Strategy |
|-----------|-----------|---------|-----------------|
| API Connectors | Custom adapters per source | Fetch feedback from 6 channels | One adapter per source; add new sources without pipeline changes |
| Normalizer | Python/Node.js | Map diverse formats to common schema | Schema versioning; backward-compatible migrations |
| Deduplication | Fuzzy matching + hash | Prevent counting same feedback twice | Exact hash for duplicates; cosine similarity for near-duplicates |
| Queue | Redis Streams or SQS | Buffer between ingestion and processing | Horizontal scaling; dead letter queue for failures |

### Common Feedback Schema
```json
{
  "id": "uuid",
  "source": "zendesk|appstore|g2|survey|gong|community",
  "source_id": "original-id-from-source",
  "text": "raw feedback text",
  "customer_id": "anonymous-customer-hash",
  "customer_segment": "enterprise|mid-market|smb",
  "product": "sailthru|cheetah|selligent",
  "timestamp": "2026-02-24T10:00:00Z",
  "metadata": {
    "rating": 4,
    "ticket_priority": "high",
    "arr_band": "$100K-$500K"
  },
  "processing": {
    "classification": "feature_request",
    "sentiment_score": 0.3,
    "sentiment_confidence": 0.92,
    "entities": ["email_builder", "segmentation", "api"],
    "embedding_id": "vec-uuid"
  }
}
```

### AI Processing Pipeline
| Step | Model | Input | Output | Latency | Cost/1K items |
|------|-------|-------|--------|---------|---------------|
| Classification | Claude 3.5 Haiku | Feedback text | Category + confidence | ~200ms | ~$0.15 |
| Sentiment | Claude 3.5 Haiku | Feedback text | Score (-1 to +1) + confidence | ~200ms | ~$0.10 |
| Entity Extraction | Claude 3.5 Haiku | Feedback text + product taxonomy | Product areas, features, segments | ~300ms | ~$0.20 |
| Embedding | Voyage-3 or text-embedding-3-small | Feedback text | 1024-dim vector | ~100ms | ~$0.02 |
| Brief Generation | Claude 3.5 Sonnet | Theme clusters + scores | Weekly intelligence brief | ~3s | ~$0.50/brief |

**Cost estimate at scale:** 5,000 feedback items/month = ~$25/month in API costs

### Intelligence Layer
| Component | Algorithm | Configuration |
|-----------|----------|---------------|
| Clustering | HDBSCAN | min_cluster_size=5, min_samples=3, metric=cosine |
| Scoring | Weighted composite | Frequency (30%) + Sentiment intensity (25%) + Revenue impact (25%) + Recency (20%) |
| Trend detection | Week-over-week delta | Alert if theme volume increases >50% or sentiment drops >0.3 |
| Alerting | Threshold monitor | Configurable per-theme; default: >20 mentions in 48 hours |

---

## Infrastructure

### Deployment
- **Container orchestration:** Docker Compose (MVP) → Kubernetes (scale)
- **Hosting:** Vercel (dashboard) + Railway/Render (backend) or self-hosted VPS
- **CI/CD:** GitHub Actions
- **Monitoring:** Sentry (errors) + custom metrics dashboard

### Security & Compliance
- All feedback anonymized before LLM processing (no PII in API calls)
- API keys stored in environment variables (never in code)
- Data encrypted at rest (PostgreSQL) and in transit (TLS)
- SOC 2 Type II alignment for enterprise deployment
- GDPR: right to deletion propagated through pipeline

---

_Architecture designed for Level 2 AI capabilities with modular components that support Level 3 autonomous agent integration (per Gupta's AI Product Strategy framework)._
