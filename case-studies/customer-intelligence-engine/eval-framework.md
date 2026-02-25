# Eval Framework: AI Customer Intelligence Engine
## Following Gupta/Husain AMI Cycle

**Author:** Angel Torres | **Date:** 2026-02-24
**Framework:** "AI Evals: Everything You Need to Know" (Gupta & Hamel Husain, 2025)

---

## 1. Why Evals Matter for This Product

The Customer Intelligence Engine processes thousands of feedback items through an LLM pipeline. Unlike traditional software, LLM outputs are non-deterministic:
- A support ticket could be classified as "bug" or "feature request" depending on framing
- Sentiment scoring may miss sarcasm or cultural context
- Clustering may group unrelated feedback or split related feedback

Without systematic evaluation, we're building on quicksand. The PM trusts the weekly brief. If the brief is wrong, they make wrong decisions. Evals are how we earn and maintain that trust.

---

## 2. The Three Gulfs (Applied)

Following Husain's framework from "Who Validates the Validators?":

### Gulf 1: Data Gulf
**Challenge:** We can't manually inspect every piece of feedback entering the pipeline.
**Our approach:** Statistical sampling. Review 100 random items weekly across all sources. Track source distribution to detect ingestion failures (e.g., Zendesk connector silently failing).

### Gulf 2: Output Gulf
**Challenge:** We can't review every classification, sentiment score, and cluster assignment.
**Our approach:** Layer evaluators. Automated confidence scoring on every output. Human review on low-confidence items. Weekly quality audit on random sample.

### Gulf 3: Specification Gulf
**Challenge:** Our prompts may not capture all the nuances of what "correct" means.
**Examples of ambiguity we must resolve:**
- Is "your API is fine but documentation is terrible" positive, negative, or mixed?
- Is "I wish you had X feature" a feature request or a complaint?
- Should feedback about onboarding be clustered with "UX" or "customer success"?

**Our approach:** Build an explicit classification taxonomy with edge case examples. Update taxonomy quarterly based on AMI cycle findings.

---

## 3. Eval Operationalization

### 3.1 Background Monitoring (Passive)
These evals run continuously without interrupting the pipeline:

| Eval | What It Measures | Trigger | Action |
|------|-----------------|---------|--------|
| Ingestion completeness | % of source items successfully ingested | Source count vs ingested count diverges >5% | Alert: "Zendesk connector may be failing" |
| Classification distribution drift | Category distribution shifts significantly | Week-over-week distribution change >15% | Flag for human review: "Are we seeing more bugs, or is classification drifting?" |
| Sentiment calibration | Average sentiment score trends | Mean sentiment shifts >0.2 without clear cause | Investigate: model behavior change or real trend? |
| Embedding quality | Intra-cluster cosine similarity | Average similarity drops below 0.7 | Re-tune clustering parameters |
| Latency monitoring | Processing time per item | P95 latency exceeds 5 seconds | Investigate API throttling or prompt bloat |

### 3.2 Guardrails (In-Path)
These evals run in the critical path and can block or modify outputs:

| Guardrail | Threshold | Action When Triggered |
|-----------|-----------|----------------------|
| Classification confidence | < 0.7 | Route to human review queue (not included in automated brief) |
| Sentiment confidence | < 0.6 | Flag as "uncertain sentiment" in data; exclude from sentiment aggregations |
| PII detection | Any PII detected in processed output | Strip PII before storage; log incident |
| Hallucination check | Brief references non-existent feedback IDs | Block brief delivery; regenerate from verified data |
| Token budget | Single item exceeds 4K tokens input | Truncate with summary prefix; log for prompt optimization |

### 3.3 Pipeline Improvement Evals
These evals generate data that improves the system:

| Eval | Purpose | Cadence |
|------|---------|---------|
| Human classification audit | Label 100 items manually; compare to system | Weekly |
| Cluster coherence review | Human judges rate cluster membership quality | Weekly |
| Brief actionability survey | PM rates each brief section as useful/not useful | Per brief (weekly) |
| Few-shot example selection | Identify misclassified items for prompt examples | After each audit |
| Edge case collection | Catalog ambiguous items for taxonomy refinement | Ongoing |

---

## 4. The AMI Cycle (Applied)

### Cycle 1: Launch (Weeks 1-4)

**ANALYZE:**
- Process 500 historical feedback items through the pipeline
- Manually review all 500 outputs
- Identify failure modes: What does the system get wrong?
- Expected findings: misclassification of mixed-sentiment items, entity extraction missing product names in abbreviations, clustering too coarse

**MEASURE:**
- Classification accuracy: target >80% (launch baseline)
- Sentiment accuracy: target >85%
- Entity extraction recall: target >70%
- Cluster coherence: target >75% human agreement

**IMPROVE:**
- Add few-shot examples for the top 10 failure cases to classification prompt
- Build product name alias dictionary for entity extraction
- Adjust HDBSCAN parameters based on coherence scores
- Refine sentiment prompt to handle mixed-sentiment items explicitly

### Cycle 2: Calibration (Weeks 5-8)

**ANALYZE:**
- Expand to 1,000 items reviewed (100/week ongoing)
- Focus on cross-channel patterns: does the system correctly correlate similar feedback from different sources?
- Identify new failure modes from real-world operation

**MEASURE:**
- Classification accuracy: target >85%
- Cross-channel correlation accuracy: target >75%
- Brief actionability rating: target >60% "useful"
- Alert precision: target >70% (low false positive rate)

**IMPROVE:**
- Tune scoring weights based on PM feedback on brief relevance
- Add new classification categories if recurring themes don't fit taxonomy
- Optimize prompt for brief generation based on PM preferences
- Implement few-shot selection pipeline: automatically pick best examples from corrected items

### Cycle 3: Maturity (Weeks 9-12)

**ANALYZE:**
- Full pipeline audit: ingestion through delivery
- PM interviews: what's working, what's missing, what's noisy?
- Cost analysis: are we processing efficiently?

**MEASURE:**
- Classification accuracy: target >90%
- Brief actionability: target >70% "useful"
- PM time savings: target >50% reduction
- Alert precision: target >80%
- Cost per insight: target <$0.05

**IMPROVE:**
- Consider model downgrades for classification (Haiku → local model) if accuracy supports it
- Build automated retraining pipeline for classification prompts
- Expand taxonomy based on 12 weeks of operational data
- Implement A/B testing on brief formats

---

## 5. Eval Metrics Dashboard

### Primary Metrics (Reviewed Weekly)
| Metric | Measurement Method | Target | Red Flag |
|--------|-------------------|--------|----------|
| Classification accuracy | Human audit sample (100/week) | >85% | <75% |
| Sentiment accuracy | Human audit sample | >90% | <80% |
| Cluster coherence | Human rating of membership | >80% | <65% |
| Brief actionability | PM survey (1-5 scale) | >3.5 avg | <2.5 avg |
| Ingestion completeness | Source vs ingested count | >95% | <85% |
| Alert precision | True positive / (true positive + false positive) | >80% | <60% |

### Secondary Metrics (Reviewed Monthly)
| Metric | Measurement Method | Target |
|--------|-------------------|--------|
| Entity extraction recall | Manual review of extracted entities | >80% |
| Processing latency (P95) | System monitoring | <5 seconds |
| Cost per feedback item | API billing / items processed | <$0.05 |
| Unique themes identified | Cluster count trending | Stable or growing |
| PM engagement | Brief open rate + dashboard logins | >80% open rate |

---

## 6. Eval Infrastructure

### Human Review Interface
- Simple web form: shows feedback text, system classification, sentiment score
- Reviewer selects correct classification and sentiment
- Disagreements logged automatically
- 10 minutes/day for a PM to review 20 items

### Automated Eval Pipeline
```
Daily:
  - Run background monitoring evals
  - Flag anomalies to Slack
  
Weekly:
  - Generate 100-item audit sample
  - Compare human labels to system labels
  - Calculate accuracy metrics
  - Update dashboard
  
Monthly:
  - Full AMI cycle review
  - Prompt optimization based on accumulated corrections
  - Cost and efficiency analysis
```

### Data Versioning
- Every prompt version is tracked with git
- Every eval run is logged with: date, sample size, metrics, prompt version
- Enables regression detection: "accuracy dropped after prompt change on Feb 15"

---

## 7. What This Demonstrates (Portfolio Value)

| AI PM Skill | How This Eval Framework Shows It |
|-------------|----------------------------------|
| AI Evals | Full AMI cycle implementation, not just "we'll test it" |
| Quality thinking | Specific metrics with targets and red flags, not vague KPIs |
| Governance | Guardrails that protect users from bad AI outputs |
| Systematic improvement | Each cycle builds on the last with measurable progress |
| Cost awareness | Efficiency metrics prevent runaway API spend |
| User-centricity | PM actionability is the ultimate metric, not just technical accuracy |

---

_Eval framework follows Aakash Gupta & Hamel Husain's "AI Evals: Everything You Need to Know" (2025), applying the Three Gulfs model and Analyze-Measure-Improve cycle._
