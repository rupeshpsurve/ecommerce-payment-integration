# PSD2/SCA Requirements Document
### Strong Customer Authentication | 3D Secure v2 | FCA Compliance

---

## 📌 Purpose

This document defines the PSD2/SCA (Strong Customer Authentication) requirements 
identified and documented during the e-commerce payment integration project for 
Euronics International Ltd across 8 European markets.

A gap analysis revealed PSD2/SCA requirements were missing from the original project 
scope. A formal change request was raised, approved, and these requirements were 
documented and implemented — resulting in a passed FCA readiness review with zero findings.

---

## 📖 Background

| Item | Detail |
|---|---|
| **PSD2** | EU Payment Services Directive 2 — regulatory framework for electronic payments |
| **SCA** | Strong Customer Authentication — requires 2 of 3 factors: something you know, have, or are |
| **3DS2** | 3D Secure version 2 — the technical standard implementing SCA for card payments |
| **Scope** | All card payments above £30 across 8 European markets |
| **Regulator** | FCA (UK), national competent authorities per market |

---

## 🔍 Gap Analysis Findings

| # | Gap Identified | Risk | Resolution |
|---|---|---|---|
| 1 | No SCA authentication flow defined in original scope | FCA non-compliance; payments rejected at checkout | Change request raised and approved |
| 2 | 3DS2 frictionless and challenge flows not documented | Development team had no spec to build against | Full 3DS2 requirements documented |
| 3 | SCA exemptions not defined | All payments would trigger challenge flow — poor UX | Exemption rules documented per market |
| 4 | Fallback logic missing | Failed 3DS2 attempts had no defined handling | Fallback process documented |
| 5 | No definition of recurring payment SCA rules | Recurring payments incorrectly triggering SCA | Merchant-initiated transaction rules added |

---

## ✅ 3DS2 Authentication Requirements

### 1. Frictionless Flow

| Req ID | Requirement | Priority |
|---|---|---|
| SCA-001 | System shall support 3DS2 frictionless flow where issuer authenticates without customer interaction | Must Have |
| SCA-002 | Frictionless flow shall be attempted first for all in-scope transactions | Must Have |
| SCA-003 | System shall pass device fingerprint and transaction risk data to issuer during frictionless attempt | Must Have |
| SCA-004 | If frictionless authentication succeeds, payment shall proceed without customer challenge | Must Have |

### 2. Challenge Flow

| Req ID | Requirement | Priority |
|---|---|---|
| SCA-005 | System shall support 3DS2 challenge flow when issuer requests additional customer verification | Must Have |
| SCA-006 | Challenge flow shall support OTP (one-time passcode) via SMS and authenticator app | Must Have |
| SCA-007 | Challenge screen shall be mobile-responsive and display within the checkout journey | Must Have |
| SCA-008 | Customer shall have maximum 3 attempts to complete the challenge before payment is declined | Must Have |
| SCA-009 | Challenge timeout shall be set to 10 minutes — payment declined if not completed | Must Have |

### 3. SCA Exemptions

| Exemption Type | Rule | Req ID |
|---|---|---|
| Low-value exemption | Transactions below £30 exempt from SCA | SCA-010 |
| Recurring payments | Subsequent recurring payments after initial SCA-authenticated setup | SCA-011 |
| Merchant-initiated transactions | Payments initiated by merchant with prior customer authorisation | SCA-012 |
| Transaction Risk Analysis (TRA) | Low-risk transactions as assessed by acquirer TRA engine | SCA-013 |
| Trusted beneficiary | Customer has whitelisted the merchant with their issuer | SCA-014 |

**Detailed Exemption Requirements:**

| Req ID | Requirement | Priority |
|---|---|---|
| SCA-010 | System shall apply low-value exemption for transactions below £30 — no SCA challenge triggered | Must Have |
| SCA-011 | System shall flag subsequent recurring payments as MIT (merchant-initiated) — SCA exemption applied | Must Have |
| SCA-012 | System shall pass exemption flag in 3DS2 authentication request to issuer | Must Have |
| SCA-013 | If exemption is rejected by issuer, system shall automatically trigger full SCA challenge flow | Must Have |
| SCA-014 | Exemption usage shall be logged per transaction for audit and reporting purposes | Must Have |

### 4. Fallback Logic

| Req ID | Requirement | Priority |
|---|---|---|
| SCA-015 | If 3DS2 authentication fails, system shall attempt 3DS1 fallback before declining | Must Have |
| SCA-016 | If both 3DS2 and 3DS1 fail, payment shall be declined with response code 402 | Must Have |
| SCA-017 | Customer shall be shown a clear decline message with guidance to contact their bank | Must Have |
| SCA-018 | All fallback attempts shall be logged with reason code for reporting | Must Have |

---

## 🔄 3DS2 Authentication Flow
Customer initiates payment at checkout
↓
Device fingerprint collected (passive)
↓
3DS2 authentication request sent to issuer
↓
┌─────┴─────┐
Frictionless   Challenge
(issuer       requested
approves)          ↓
↓        Customer completes
Payment       OTP / biometric
authorised         ↓
┌───┴───┐
Pass      Fail
↓          ↓
Payment    3DS1 fallback
authorised      ↓
┌────┴────┐
Pass      Fail
↓          ↓
Payment    Payment
authorised  declined (402)

---

## 📋 Acceptance Criteria (Key Examples)

**SCA-001 — Frictionless flow**
- Given a customer initiates a card payment above £30
- When the 3DS2 authentication request is sent to the issuer
- Then the system shall first attempt frictionless authentication
- And only trigger challenge flow if the issuer requests it

**SCA-010 — Low-value exemption**
- Given a customer initiates a card payment below £30
- When the payment is processed
- Then SCA shall not be triggered and the payment shall proceed without challenge

**SCA-013 — Exemption rejected by issuer**
- Given the system requests a TRA exemption for a transaction
- When the issuer rejects the exemption request
- Then the system shall automatically trigger the full 3DS2 challenge flow
- And shall not decline the payment without first attempting authentication

---

## 🌍 Market-Specific Considerations

| Market | Currency | SCA Threshold | Notes |
|---|---|---|---|
| UK | GBP | £30 | FCA regulated post-Brexit |
| Germany | EUR | €30 | BaFin regulated |
| France | EUR | €30 | ACPR regulated |
| Netherlands | EUR | €30 | DNB regulated |
| Italy | EUR | €30 | Banca d'Italia regulated |
| Spain | EUR | €30 | Banco de España regulated |
| Belgium | EUR | €30 | NBB regulated |
| Poland | PLN | 150 PLN | KNF regulated |

---

## 📈 Outcome

| Metric | Result |
|---|---|
| FCA readiness review | Passed with **zero findings** |
| PSD2/SCA implementation | Fully compliant across all 8 markets |
| Change request | Approved and delivered within project timeline |

---

## 📁 Related Documents

- [Payment Lifecycle Requirements](./payment-lifecycle-requirements.md)
- [API Business Requirements](../api/api-business-requirements.md)
- [Reconciliation Rules](../reconciliation/reconciliation-rules.md)

---

*Produced by Rupesh Surve | Business Analyst | Part of E-commerce Payment Integration Portfolio*
