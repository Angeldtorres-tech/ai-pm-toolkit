# AI Governance Checklist

A comprehensive checklist for responsible AI deployment. Use this before launching any AI feature or integrating an LLM into production.

---

## 🔍 1. LLM / Model Evaluation

- [ ] Model candidates evaluated against task-specific benchmarks
- [ ] Accuracy, latency, and cost compared across providers
- [ ] Model version pinned (not "latest") for reproducibility
- [ ] Evaluation dataset created that represents real-world usage
- [ ] Edge cases and adversarial inputs tested
- [ ] Model card reviewed for known limitations
- [ ] Licensing terms compatible with intended use

## 🛡️ 2. Guardrails & Safety

- [ ] Input validation implemented (prompt injection defense)
- [ ] Output filtering for harmful/inappropriate content
- [ ] Token/length limits enforced
- [ ] Rate limiting configured per user/session
- [ ] Jailbreak/prompt injection testing completed
- [ ] Content moderation layer in place
- [ ] PII detection and redaction in prompts and responses
- [ ] Fallback behavior defined when model is unavailable

## 👤 3. Human-in-the-Loop

- [ ] Defined which decisions require human review
- [ ] Escalation path documented for uncertain/high-stakes outputs
- [ ] Human override mechanism available
- [ ] Feedback loop for users to flag incorrect outputs
- [ ] Review queue for edge cases and low-confidence results
- [ ] Audit trail of human interventions maintained

## ⚠️ 4. Risk Assessment

- [ ] Risk matrix completed (likelihood × impact)
- [ ] Failure modes identified and documented
- [ ] Worst-case scenario analysis performed
- [ ] Rollback plan defined and tested
- [ ] Incident response plan for AI-specific failures
- [ ] Legal review completed for regulated industries
- [ ] Insurance/liability implications assessed

## 🔒 5. Data Privacy

- [ ] Data classification completed (PII, PHI, confidential)
- [ ] Data processing agreements in place with AI providers
- [ ] User consent mechanisms implemented
- [ ] Data retention policy defined and enforced
- [ ] Right to deletion/GDPR compliance verified
- [ ] Cross-border data transfer requirements met
- [ ] Training data sourced ethically and legally
- [ ] No customer data used for model training without consent

## 📊 6. Bias & Fairness Monitoring

- [ ] Bias evaluation performed across demographic groups
- [ ] Fairness metrics selected (demographic parity, equalized odds, etc.)
- [ ] Test dataset includes diverse representation
- [ ] Regular bias audits scheduled (quarterly minimum)
- [ ] Disparate impact analysis completed
- [ ] Mitigation strategies documented for identified biases
- [ ] Third-party bias audit considered for high-stakes applications

## 📝 7. Transparency & Documentation

- [ ] AI disclosure to end users (they know they're interacting with AI)
- [ ] Model behavior documented in plain language
- [ ] Decision explanation capability (why did the AI say X?)
- [ ] System architecture documented
- [ ] Change log maintained for prompt/model updates
- [ ] Public-facing AI usage policy published

## 📈 8. Monitoring & Observability

- [ ] Response quality metrics tracked in production
- [ ] Drift detection configured (input distribution, output quality)
- [ ] Cost monitoring with alerts for anomalies
- [ ] Latency monitoring with SLA thresholds
- [ ] Error rate tracking and alerting
- [ ] User satisfaction / feedback tracking
- [ ] Regular model performance reviews scheduled

## ✅ 9. Launch Readiness

- [ ] All critical checklist items above completed
- [ ] Staged rollout plan (canary → beta → GA)
- [ ] Kill switch / feature flag implemented
- [ ] On-call rotation defined for AI-related incidents
- [ ] Success metrics and evaluation criteria agreed upon
- [ ] Post-launch review scheduled (30/60/90 day)

---

## Sign-Off

| Role | Name | Date | Approved |
|------|------|------|----------|
| Product Manager | | | ☐ |
| Engineering Lead | | | ☐ |
| Data/ML Lead | | | ☐ |
| Legal/Compliance | | | ☐ |
| Security | | | ☐ |
