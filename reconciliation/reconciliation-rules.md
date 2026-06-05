# Reconciliation Rules & Duplicate Detection
### Payment Reconciliation Framework | Duplicate Detection | Invoice Matching

---

## 📌 Purpose

This document defines the reconciliation rules and duplicate payment detection logic 
designed for the Euronics International Ltd e-commerce payment integration project 
across 8 European markets.

These rules reduced unreconciled transactions by 35% within 3 months of go-live and 
resolved an EUR 47,000 undetected duplicate payment problem.

---

## 📖 Background

| Item | Detail |
|---|---|
| **Problem** | Payments captured but not matched to invoices; duplicate payments going undetected |
| **Impact before** | EUR 47,000 in undetected duplicate payments; high volume of unreconciled transactions |
| **Solution** | Structured reconciliation rules and automated duplicate detection logic |
| **Result** | 35% reduction in unreconciled transactions within 3 months of go-live |

---

## 🔁 Reconciliation Process Overview
Payment Captured (payment.captured webhook received)
↓
Match payment to order reference
↓
┌─────┴─────┐
Match        No Match
found           ↓
↓         Flag for manual
Validate        review queue
amount            ↓
↓         Finance team
┌──┴──┐       investigates
Match  Discrepancy
↓        ↓
Mark    Flag and
settled  alert
---

## ✅ Reconciliation Rules

### 1. Payment to Order Matching

| Rule ID | Rule | Action if Failed |
|---|---|---|
| REC-001 | Every captured payment shall be matched to an order using the merchant reference field | Flag as unmatched — route to finance review queue |
| REC-002 | Merchant reference format shall follow: ORDER-{MARKET}-{YEAR}-{SEQUENCE} (e.g. ORDER-EU-2023-4521) | Reject payment initiation if format invalid |
| REC-003 | Matched payment amount shall equal the order invoice amount exactly | Flag discrepancy if difference exceeds €0.01 (rounding tolerance) |
| REC-004 | Currency of payment shall match currency of invoice | Flag currency mismatch — route to finance review |
| REC-005 | Each order reference shall be matched to exactly one captured payment | Flag if multiple payments matched to same order |

### 2. Settlement Reconciliation

| Rule ID | Rule | Action if Failed |
|---|---|---|
| REC-006 | Daily settlement file from acquirer shall be reconciled against captured payments each business day | Alert finance team of any unreconciled items by 09:00 |
| REC-007 | Settled amount shall match captured amount for each transaction | Flag discrepancy above €10 for finance review |
| REC-008 | Settlement date shall be within 3 business days of capture date | Flag delayed settlements for follow-up with acquirer |
| REC-009 | All settlement discrepancies shall be resolved within 5 business days | Escalate to Finance Manager if unresolved after 5 days |
| REC-010 | Multi-currency settlements shall be reconciled using the exchange rate on the settlement date | Log exchange rate used for audit purposes |

### 3. Refund Reconciliation

| Rule ID | Rule | Action if Failed |
|---|---|---|
| REC-011 | Refund amount shall not exceed the original captured payment amount | Block refund and alert finance team |
| REC-012 | Partial refunds shall be tracked against the original order — cumulative refunds shall not exceed original amount | Block if cumulative refunds would exceed original payment |
| REC-013 | Refund shall be matched to original payment using payment ID | Reject refund request if original payment not found |
| REC-014 | Refund settlement shall be reconciled against refund initiation within 10 business days | Flag delayed refund settlements for acquirer follow-up |

---

## 🚫 Duplicate Payment Detection Rules

### Detection Logic

| Rule ID | Rule | Action |
|---|---|---|
| DUP-001 | Payment shall be flagged as duplicate if same order reference is submitted more than once | Block second payment — return HTTP 409 |
| DUP-002 | Payment shall be flagged as duplicate if same card, same amount, same merchant within 60 seconds | Block and flag for finance review |
| DUP-003 | Payment shall be flagged as suspicious if same card submits 3 or more payments within 10 minutes | Flag for fraud review — do not auto-block |
| DUP-004 | Blocked duplicate payments shall be logged with: original payment ID, duplicate attempt timestamp, reason code | Logged for audit and reporting |
| DUP-005 | Finance team shall receive daily report of all duplicate detections and blocks | Daily 08:00 report |

### Duplicate Detection Decision Matrix

| Scenario | Same Order Ref | Same Card | Same Amount | Within 60 secs | Action |
|---|---|---|---|---|---|
| Clear duplicate | ✅ | ✅ | ✅ | ✅ | Block — HTTP 409 |
| Likely duplicate | ❌ | ✅ | ✅ | ✅ | Block — HTTP 409 |
| Suspicious | ❌ | ✅ | ✅ | ❌ | Flag for review |
| Legitimate retry | ❌ | ✅ | ✅ | ❌ | Allow — log only |
| Different customer | ❌ | ❌ | ✅ | ✅ | Allow — log only |

---

## 📋 Acceptance Criteria (Key Examples)

**REC-001 — Payment to order matching**
- Given a payment.captured webhook is received
- When the system processes the captured payment
- Then the system shall attempt to match the merchant reference to an existing order
- And if no match is found, flag the payment in the finance review queue within 15 minutes

**REC-007 — Settlement discrepancy**
- Given the daily settlement file is received from the acquirer
- When the system reconciles settled amounts against captured amounts
- Then any discrepancy greater than €10 shall be flagged in the finance dashboard
- And the finance team shall be alerted by email within 1 hour

**DUP-001 — Duplicate order reference**
- Given a payment initiation request is received
- When the merchant reference matches an already captured payment
- Then the system shall block the payment and return HTTP 409
- And log the duplicate attempt with timestamp and original payment ID

**DUP-002 — Same card same amount**
- Given a payment is captured successfully
- When another payment is initiated with the same card and same amount within 60 seconds
- Then the system shall block the second payment and return HTTP 409

---

## 📊 Reconciliation Reporting Requirements

| Report | Frequency | Audience | Content |
|---|---|---|---|
| Daily reconciliation summary | Daily 09:00 | Finance Team | Matched, unmatched, discrepancies |
| Settlement discrepancy report | Daily 09:00 | Finance Team | Discrepancies above €10 |
| Duplicate detection report | Daily 08:00 | Finance Team | All duplicate blocks and flags |
| Unresolved items report | Weekly Monday | Finance Manager | Items unresolved beyond SLA |
| Monthly reconciliation report | 1st of each month | CFO | Summary of reconciliation health |

---

## 📈 Results & Impact

| Metric | Before | After |
|---|---|---|
| Unreconciled transactions | High volume — manual process | Reduced by **35%** within 3 months |
| Duplicate payment detection | No automated detection | EUR 47,000 problem resolved |
| Settlement discrepancy resolution | Ad hoc, no SLA | 5 business day SLA defined |
| Reconciliation reporting | Manual Excel | Automated daily reports |

---

## 📁 Related Documents

- [Payment Lifecycle Requirements](../requirements/payment-lifecycle-requirements.md)
- [PSD2/SCA Requirements](../requirements/psd2-sca-requirements.md)
- [API Business Requirements](../api/api-business-requirements.md)

---

*Produced by Rupesh Surve | Business Analyst | Part of E-commerce Payment Integration Portfolio*
