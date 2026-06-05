# API Business Requirements
### Payment Gateway REST API | Endpoints | Response Codes | Webhooks

---

## 📌 Purpose

This document translates the payment gateway REST API technical documentation into 
business requirements for the Euronics International Ltd e-commerce payment 
integration project across 8 European markets.

Produced to bridge the gap between the vendor's technical API documentation and the 
business and development teams' understanding of integration requirements.

---

## 📖 API Overview

| Item | Detail |
|---|---|
| **API Type** | REST API |
| **Authentication** | OAuth 2.0 Bearer Token |
| **Data Format** | JSON |
| **Environments** | Sandbox (testing), Production |
| **Versioning** | API version included in base URL (e.g. /v2/) |
| **Rate Limiting** | 100 requests per second per merchant account |

---

## 🔗 API Endpoints — Business Requirements

### 1. Payment Initiation

| Req ID | Endpoint | Method | Business Requirement |
|---|---|---|---|
| API-001 | /v2/payments | POST | System shall call this endpoint to initiate a new payment authorisation request |
| API-002 | /v2/payments/{id} | GET | System shall call this endpoint to retrieve current status of an existing payment |
| API-003 | /v2/payments/{id}/captures | POST | System shall call this endpoint to capture an authorised payment |
| API-004 | /v2/payments/{id}/refunds | POST | System shall call this endpoint to initiate a full or partial refund |
| API-005 | /v2/payments/{id}/cancels | POST | System shall call this endpoint to cancel an authorised but uncaptured payment |

### 2. Request Parameters — Payment Initiation (API-001)

| Parameter | Type | Required | Business Rule |
|---|---|---|---|
| `amount` | Integer | Yes | Amount in minor units (e.g. £10.00 = 1000) |
| `currency` | String | Yes | ISO 4217 currency code (GBP, EUR, PLN) |
| `reference` | String | Yes | Unique merchant order reference — used for reconciliation |
| `description` | String | Yes | Payment description shown on customer statement |
| `customer.email` | String | Yes | Customer email for payment confirmation |
| `customer.name` | String | Yes | Customer full name |
| `payment_method` | String | Yes | Card type or local payment method (e.g. ideal, blik) |
| `capture_mode` | String | Yes | immediate or delayed |
| `3ds.enabled` | Boolean | Yes | Must be true for all in-scope transactions |
| `metadata` | Object | No | Additional merchant data for reporting |

---

## 📨 HTTP Response Code Mapping

| Response Code | Meaning | Business Action | Customer Message |
|---|---|---|---|
| 200 OK | Payment authorised successfully | Proceed to capture | "Payment successful" |
| 201 Created | Payment resource created | Await authorisation response | — |
| 400 Bad Request | Invalid request parameters | Log error; alert development team | "Payment unavailable — please try again" |
| 402 Payment Required | Payment declined by issuer | Log decline reason; offer retry | "Payment declined — please try another card" |
| 409 Conflict | Duplicate payment detected | Block payment; log for finance review | "This payment has already been processed" |
| 422 Unprocessable | Invalid card details | Prompt customer to correct details | "Please check your card details and try again" |
| 429 Too Many Requests | Rate limit exceeded | Implement exponential backoff retry | "Payment unavailable — please try again shortly" |
| 500 Server Error | Gateway internal error | Retry after 30 seconds; alert if persists | "Payment unavailable — please try again shortly" |

---

## 📡 Webhook Event Requirements

Webhooks are event notifications sent by the payment gateway to Euronics systems 
when key payment events occur.

### Webhook Endpoint Requirements

| Req ID | Requirement | Priority |
|---|---|---|
| WH-001 | Euronics shall expose a webhook endpoint to receive payment gateway event notifications | Must Have |
| WH-002 | Webhook endpoint shall return HTTP 200 within 5 seconds to acknowledge receipt | Must Have |
| WH-003 | If acknowledgement not received, gateway shall retry webhook up to 3 times at 5-minute intervals | Must Have |
| WH-004 | All webhook payloads shall be validated using HMAC signature verification | Must Have |
| WH-005 | Duplicate webhook events shall be detected and ignored using event ID deduplication | Must Have |

### Webhook Events — Business Requirements

| Event | Trigger | Business Action | Req ID |
|---|---|---|---|
| `payment.authorised` | Payment approved by issuer | Trigger capture request | WH-006 |
| `payment.captured` | Funds successfully captured | Update order status to confirmed; send confirmation email | WH-007 |
| `payment.failed` | Payment declined or failed | Update order status to failed; notify customer | WH-008 |
| `payment.cancelled` | Authorised payment cancelled | Release reserved
