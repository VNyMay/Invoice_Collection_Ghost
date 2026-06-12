# Collection Ghost – AI-Powered AR Automation & Dunning SaaS

Collection Ghost is a production-ready, AI-powered accounts receivable (AR) automation and dunning SaaS boilerplate.

It is designed for B2B companies that struggle with unpaid invoices, late payments, and high Days Sales Outstanding (DSO). The system automates invoice tracking, escalation of overdue accounts, and the drafting and sending of collection emails.

This repository currently serves as a documentation and project overview.  
The complete source code (frontend, backend, database, AI and email integrations) is part of the asset being sold.

---

## 1. Overview

Collection Ghost provides a structured, end-to-end workflow for managing overdue invoices:

- Tracks invoices, due dates, and days overdue.
- Assigns urgency levels (for example, Friendly, Firm, Final).
- Generates collection emails using an AI model (Gemini 3.5) with a well-defined JSON output structure.
- Sends emails through Resend, or records them in a local log when no email provider is configured.
- Includes an ROI calculator that estimates recovered cash and the impact on DSO.
- Exposes a modern dashboard for finance or AR staff to work with.

The goal is to give a buyer a ready-made foundation for a commercial AR automation product, rather than a simple demo or prototype.

---

## 2. Architecture

### Frontend

- React (with Vite) single-page application.
- Tailwind CSS for layout and styling.
- Main interface components include:
  - An aging arrears ledger showing invoices and their status.
  - An AI email drafting and preview panel.
  - An ROI calculator for estimating financial impact.
  - A log view for system and email events.

### Backend

- Node.js with Express as the API server.
- Multi-tenant support via a `tenant_id` field and an `x-tenant-id` HTTP header.
- Symmetric cryptographic vault based on AES-256-GCM for encrypting:
  - Resend API keys.
  - Email provider credentials.
  - Other sensitive configuration values.
- SQLite database in WAL mode for reliable, concurrent reads and writes.

### AI and Automation

- Integration with Gemini 3.5 via the `@google/genai` SDK.
- An AI drafting endpoint that:
  - Accepts invoice and debtor information.
  - Applies a tone (Friendly, Firm, or Final).
  - Returns JSON with `subject`, `body`, and an array of `recoveryTips`.
- Provides a fallback mode when no AI key is configured, so the system can still be demonstrated locally.

### Email and External Integrations

- Resend API for sending production emails.
- Sandbox mode that logs outgoing messages to the database when no email key is available, preventing runtime failures during development.
- Database schema and logic prepared for Stripe and QuickBooks integrations:
  - Synchronisation of invoices and customer details.
  - Storage of provider references and URLs.

---

## 3. Data Model (High-Level)

The database schema is designed around a multi-tenant pattern. At a high level, it includes:

- `tenants` – records for each tenant or company.
- `tenant_settings` – configuration per tenant (company name, support email, API keys, etc.).
- `dunning_templates` – templates for Friendly, Firm, and Final messages.
- `invoices` – invoice records with amount due, currency, days overdue, current status, and links to online invoice pages.
- `jobs` – background job records (for example, ledger synchronisation tasks).
- `logs` – a log of system events, including sandboxed email deliveries.
- `oauth_tokens` – storage for access and refresh tokens for external APIs.

Monetary amounts are stored as integers in minor units (for example, cents) to avoid floating-point rounding errors.

---

## 4. Technology Stack

- Frontend: React, Vite, Tailwind CSS
- Backend: Node.js, Express, TypeScript
- Database: SQLite (WAL mode enabled)
- AI: Gemini 3.5 via `@google/genai` SDK
- Email: Resend (with a local sandbox fallback)
- Background processing: Python daemon for ledger parsing and synchronisation (for example, Stripe and QuickBooks)

---

## 5. Runtime Design (Conceptual Flow)

1. The frontend sends HTTP requests to `/api/...` endpoints, including an `x-tenant-id` header to identify the tenant.
2. The Express backend:
   - Resolves the tenant context.
   - Reads from and writes to the SQLite database using tenant-scoped queries.
3. The AI drafting endpoint (`/api/draft-ai-email`):
   - Receives invoice and debtor information.
   - Calls Gemini 3.5 (or uses a safe fallback when no API key is present).
   - Returns JSON containing the email subject, body, and recovery tips.
4. The email sending endpoint (`/api/invoices/send-custom`):
   - If a valid Resend API key exists, sends the email to the debtor.
   - If not, records a sandbox event in the `logs` table and marks the invoice as having a reminder sent.
5. A Python daemon runs separately to synchronise external ledger data and update the `invoices` and `jobs` tables.

The system is intended to be container-friendly and suitable for deployment on common platforms such as Cloud Run or similar services.

---

## 6. What Is for Sale

The following are included in the sale:

- Full source code:
  - Frontend (React/Vite/Tailwind).
  - Backend (Express/TypeScript).
  - SQLite schema and initialisation scripts.
  - Python daemon for external ledger synchronisation.
- AI integration logic (Gemini 3.5) with structured JSON enforcement.
- Email integration logic with Resend and a safe sandbox mode.
- The ROI calculator and the main analytics and logging views.
- Deployment and configuration instructions sufficient to run the system in a production-like environment.

This repository is only a public, high-level overview. The full implementation is provided to the buyer as part of the transfer.

---

## 7. Sale Details

- Asset: Collection Ghost – AI-Powered AR Automation & Dunning SaaS
- Type of sale: Source code and full rights (exclusive transfer)
- Stage: Pre-revenue, no active users
- Asking price: 5,000 USD
- Lowest acceptable price: 3,000 USD (negotiable)

This asset is suitable for:

- AR automation or fintech startups that want to accelerate product development.
- B2B SaaS providers that wish to add an AR/dunning module.
- Agencies that build finance or back-office systems for clients and want a reusable AR engine.

---

## 8. Contact

For enquiries about acquiring this project, please contact:

- Email: your-email@example.com

(Replace this with your actual email address before publishing.)
