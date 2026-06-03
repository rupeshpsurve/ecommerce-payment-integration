# E-commerce Payment Integration & Reconciliation
### Payment Gateway Integration | PSD2/SCA Compliance | REST APIs | Reconciliation Framework

---

## 📌 Project Overview

This project involved gathering, documenting, and delivering end-to-end payment 
gateway requirements for **Euronics International Ltd's** e-commerce platform 
expansion across **8 European markets**.

The engagement covered the full payment lifecycle, REST API integration, PSD2/SCA 
regulatory compliance, and a reconciliation framework that reduced unreconciled 
transactions by 35% within 3 months of go-live.

---

## 🎯 Objectives

- Document end-to-end payment gateway requirements across 8 European markets
- Produce BPMN swimlane diagrams for all key payment scenarios
- Translate payment gateway REST API documentation into business requirements
- Identify and remediate PSD2/SCA compliance gaps before FCA readiness review
- Design duplicate payment detection and reconciliation rules
- Lead the payment integration workstream from requirements through to UAT sign-off

---

## 🛠️ Tools & Technologies

| Category | Details |
|---|---|
| **Payment Standards** | PSD2/SCA, 3D Secure v2 (3DS2), ISO 20022 awareness |
| **Integration** | Payment Gateway REST APIs, Webhooks, HTTP response codes |
| **Process Modelling** | BPMN, Visio, Swimlane diagrams |
| **Data** | SQL, MS Excel, Reconciliation rules |
| **Documentation** | Confluence, Jira |
| **Regulatory** | FCA readiness, SCA exemptions, AML |

---

## 🔄 My Approach (As Business Analyst)

**1. Discovery & Requirements Gathering**
- Ran discovery workshops with ecommerce, finance, and customer service teams
- Mapped end-to-end payment lifecycle across 8 European markets:
  `Initiation → Authorisation → Capture → Settlement → Refund → Chargeback`
- Produced BPMN swimlane diagrams in Visio for 6 payment scenarios

**2. API Requirements Translation**
- Translated payment gateway REST API documentation into business requirements
- Mapped HTTP response codes to business actions (200, 402, 409)
- Specified webhook events for: `payment.captured`, `payment.failed`, `chargeback.created`

**3. PSD2/SCA Compliance**
- Identified missing PSD2/SCA requirements during scope review
- Raised formal change request — approved by project board
- Documented 3D Secure v2 requirements covering:
  - Frictionless flow
  - Challenge flow
  - SCA exemptions (sub-£30, recurring payments, low-risk TRA)
  - Fallback logic

**4. Reconciliation Framework**
- Designed duplicate payment detection rules
- Defined reconciliation rules to match payments against invoices
- Resolved vendor API documentation discrepancies before development began

**5. UAT & Delivery**
- Led workstream from requirements through to UAT sign-off
- Resolved API documentation issues with vendor before integration testing
- Supported stakeholders during UAT and data validation

---

## 📊 Key Artefacts Produced

- ✅ End-to-end payment lifecycle map (8 European markets)
- ✅ BPMN swimlane diagrams for 6 payment scenarios
- ✅ REST API business requirements (endpoints, response codes, webhooks)
- ✅ PSD2/SCA gap analysis and change request
- ✅ 3D Secure v2 requirements document
- ✅ Duplicate detection and reconciliation rules
- ✅ UAT sign-off documentation

---

## 📈 Results & Impact

| Metric | Outcome |
|---|---|
| FCA readiness review | Passed with **zero findings** |
| PSD2/SCA compliance | 3DS2 fully implemented and compliant |
| Unreconciled transactions | Reduced by **35%** within 3 months of go-live |
| Duplicate payment problem | EUR 47,000 undetected payment issue resolved |
| Payment scenarios | 6 BPMN diagrams delivered as unambiguous specifications |
| API issues | Vendor discrepancies resolved before integration testing |

---

## 💡 Key Domain Knowledge Demonstrated

- **Payment lifecycle**: Initiation, Authorisation, Capture, Settlement, Refund, Chargeback
- **PSD2/SCA**: Strong Customer Authentication requirements, exemption rules, FCA compliance
- **3D Secure v2**: Frictionless flow, challenge flow, SCA exemptions, fallback logic
- **REST APIs**: Endpoint mapping, HTTP response codes, webhook event specifications
- **Reconciliation**: Duplicate detection logic, invoice matching rules
- **BPMN**: Swimlane process diagrams for complex payment scenarios

---

## 🏢 Client & Context

- **Client:** Euronics International Ltd
- **Employer:** Atos Global IT Solutions and Services
- **Role:** Business Analyst – E-commerce Payment Integration
- **Markets:** 8 European markets
- **Location:** India

---

## 📁 Repository Structure
