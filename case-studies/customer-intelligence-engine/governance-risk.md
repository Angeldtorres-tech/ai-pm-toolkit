# Governance & Risk Document: AI Customer Intelligence Engine

**Author:** Angel Torres | **Date:** 2026-02-24
**Compliance Alignment:** NIST AI RMF, SOC 2 Type II, GDPR

---

## 1. AI Governance Principles

### 1.1 Human-in-the-Loop Design
The CIE operates as an **advisory system**, not a decision-making system.

- All outputs are recommendations, never autonomous actions
- Weekly briefs are labeled "AI-Assisted Intelligence" with confidence indicators
- PMs make all prioritization and roadmap decisions
- No customer communication is triggered without human approval
- Classification below 0.7 confidence routes to human review queue

### 1.2 Transparency
- Every insight in the brief links to source feedback (verifiable)
- Confidence scores are visible, not hidden
- Brief includes "How this was generated" section explaining methodology
- Classification taxonomy is documented and accessible to all stakeholders
- Monthly accuracy reports published to the product team

### 1.3 Accountability
- System owner: Product Operations team
- Eval owner: designated PM (rotates quarterly)
- Incident response: any misclassification affecting roadmap decisions triggers a post-mortem
- Audit trail: every classification decision logged with model version, prompt version, and confidence

---

## 2. Data Governance

### 2.1 Data Classification

| Data Type | Sensitivity | Handling |
|-----------|------------|----------|
| Feedback text (anonymized) | Internal | Processed by LLM API; stored in PostgreSQL |
| Customer identifiers | Confidential | Hashed before storage; never sent to LLM API |
| ARR/revenue data | Confidential | Used for scoring only; aggregated, never individual |
| Sentiment scores | Internal | Stored in PostgreSQL; included in briefs |
| Embeddings | Internal | Stored in vector database; no PII content |
| Intelligence briefs | Internal | Distributed to authorized PM team only |

### 2.2 Data Flow Controls

**Ingestion:**
- PII stripping occurs BEFORE any data enters the processing pipeline
- Customer names replaced with anonymous hashes
- Email addresses, phone numbers, and account IDs removed
- Company names retained only when relevant to product feedback (e.g., "integrating with Salesforce")

**Processing:**
- LLM API calls contain anonymized text only
- No customer identifiers in prompts
- Batch processing prevents correlation of individual customer patterns
- All API calls over TLS 1.3

**Storage:**
- PostgreSQL encrypted at rest (AES-256)
- Vector store isolated from customer identity data
- Retention: processed data 12 months, raw ingestion logs 6 months
- Deletion: cascade delete on customer account removal (GDPR compliance)

**Delivery:**
- Briefs delivered to authenticated recipients only
- Dashboard behind SSO authentication
- No customer-identifiable data in briefs (aggregate themes only)
- Slack integration limited to designated channels

### 2.3 GDPR Compliance

| GDPR Right | Implementation |
|------------|---------------|
| Right to access | Customer can request all feedback data associated with their hashed ID |
| Right to erasure | Deletion request triggers cascade through pipeline; embeddings regenerated |
| Right to rectification | Feedback can be updated/corrected; reprocessed in next batch |
| Data minimization | Only feedback text and metadata ingested; no personal profiles |
| Purpose limitation | Data used exclusively for product intelligence; no marketing or sales use |
| Data portability | Export in standard JSON format |

---

## 3. AI-Specific Risks

### 3.1 Risk Register

| ID | Risk | Category | Likelihood | Impact | Severity | Mitigation | Owner |
|----|------|----------|-----------|--------|----------|------------|-------|
| R1 | LLM misclassifies critical feedback (e.g., security issue labeled as feature request) | Accuracy | Medium | Critical | High | Confidence guardrail at 0.7; security/compliance keywords trigger mandatory human review | Eval Owner |
| R2 | Clustering creates misleading theme groupings | Accuracy | Medium | High | High | Weekly coherence audit; human review of new clusters; anomaly detection on cluster changes | Eval Owner |
| R3 | Brief contains hallucinated statistics or fabricated trends | Integrity | Low | Critical | High | All brief data points reference specific feedback IDs; automated hallucination check before delivery | System Owner |
| R4 | Sentiment analysis fails on sarcasm/cultural context | Accuracy | High | Medium | Medium | Include edge case examples in prompt; flag low-confidence sentiment; cultural context notes in taxonomy | Eval Owner |
| R5 | PII leaks into LLM API calls | Privacy | Low | Critical | High | PII stripping before processing; regex + NER detection; quarterly PII audit | System Owner |
| R6 | Model provider (Anthropic) changes behavior after update | Stability | Medium | High | High | Pin model versions; regression test suite; monitor accuracy metrics for sudden changes | System Owner |
| R7 | Cost spike from unexpected feedback volume | Financial | Medium | Medium | Medium | Token budgets per source; batch processing; cost monitoring alerts | System Owner |
| R8 | PM over-trusts AI insights, stops validating | Behavioral | Medium | High | High | Confidence scores on every insight; monthly "trust calibration" review; encourage drill-down culture | Product Lead |
| R9 | Source API changes break ingestion | Technical | Medium | Medium | Medium | Health monitoring per source; graceful degradation (brief notes missing sources); modular adapters | System Owner |
| R10 | Adversarial feedback manipulates themes | Security | Low | Medium | Low | Anomaly detection on feedback volume spikes; source verification; outlier filtering | System Owner |

### 3.2 Risk Severity Matrix

```
              Low Impact    Medium Impact   High Impact    Critical Impact
High Likelihood                  R4
Med Likelihood      R7         R2, R9        R1, R6, R8       
Low Likelihood      R10                                     R3, R5
```

**Priority order:** R3 (hallucination) > R5 (PII) > R1 (misclassification) > R6 (model changes) > R8 (over-trust)

---

## 4. Guardrail Specifications

### 4.1 Input Guardrails
| Guardrail | Implementation | Failure Mode |
|-----------|---------------|--------------|
| PII filter | Regex + spaCy NER before LLM processing | Block item; log incident; alert |
| Length filter | Reject items > 10,000 characters | Truncate with summary; process summary |
| Language filter | Detect non-English; route to appropriate model | Queue for translation pipeline |
| Spam filter | LLM-based relevance check | Exclude from analysis; log |
| Source verification | Validate source API authentication | Reject; alert on auth failure |

### 4.2 Output Guardrails
| Guardrail | Implementation | Failure Mode |
|-----------|---------------|--------------|
| Confidence threshold | Classification < 0.7 | Route to human review; exclude from automated brief |
| Hallucination check | Verify all cited feedback IDs exist in database | Block brief; regenerate |
| Toxicity filter | Screen brief text for inappropriate content | Flag for human review |
| Consistency check | Compare brief claims to underlying data | Alert if discrepancy detected |
| Format validation | Brief meets schema requirements | Regenerate; use template fallback |

### 4.3 Operational Guardrails
| Guardrail | Implementation | Failure Mode |
|-----------|---------------|--------------|
| Cost ceiling | Daily API spend limit ($20/day) | Pause processing; alert; queue items |
| Rate limiting | Max 100 LLM calls/minute | Queue excess; process in next batch |
| Circuit breaker | 5 consecutive API failures | Pause pipeline; alert; attempt recovery after 5 min |
| Stale data detection | Brief references data > 7 days old | Warning label in brief; investigate pipeline delay |

---

## 5. Incident Response

### Severity Levels
| Level | Definition | Response Time | Example |
|-------|-----------|---------------|---------|
| P1 - Critical | PII exposure or security breach | 1 hour | Customer data sent to LLM API unmasked |
| P2 - High | Misleading insight delivered to PM team | 4 hours | Brief contains fabricated trend that influences roadmap |
| P3 - Medium | Accuracy degradation affecting brief quality | 24 hours | Classification accuracy drops below 75% |
| P4 - Low | Non-blocking issue affecting efficiency | 1 week | One source connector intermittently failing |

### Response Procedure
1. **Detect:** Automated monitoring or human report
2. **Triage:** Assign severity level
3. **Contain:** Pause affected pipeline component if P1/P2
4. **Investigate:** Root cause analysis
5. **Remediate:** Fix and verify
6. **Review:** Post-mortem for P1/P2; lessons learned doc
7. **Prevent:** Update guardrails, evals, or architecture to prevent recurrence

---

## 6. Ethical Considerations

### Bias Detection
- Monthly analysis of classification distribution by customer segment (enterprise vs SMB)
- Ensure small customers' feedback isn't systematically deprioritized
- Revenue-weighted scoring explicitly documented so PMs understand the tradeoff
- Quarterly review: "Are we hearing equally from all customer segments?"

### Transparency with Customers
- Customer-facing teams informed that feedback is analyzed by AI
- Privacy policy updated to reflect automated processing
- Customers can opt out of automated analysis (feedback still collected, manually reviewed)

### Team Impact
- CIE augments PM workflow, does not replace PMs
- Clear communication: "This saves you 4 hours/week on data processing so you can spend more time on strategy"
- PMs retain full ownership of roadmap decisions

---

_Governance framework aligned with NIST AI Risk Management Framework (2023) and informed by Gupta/Husain's eval operationalization methodology._
