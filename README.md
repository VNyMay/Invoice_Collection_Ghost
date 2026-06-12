# Collection Ghost – AI-Powered AR Automation and Dunning SaaS

Collection Ghost is a production-ready, AI-powered accounts receivable (AR) automation and dunning SaaS boilerplate.

It is designed for B2B companies that struggle with unpaid invoices, late payments, and high Days Sales Outstanding, which is often called DSO. The system automates invoice tracking, escalation of overdue accounts, and the drafting and sending of collection emails.

This repository currently serves as a documentation and project overview. The complete source code, including frontend, backend, database, AI and email integrations, is part of the asset being sold.

## 1. Overview

Collection Ghost provides a structured, end-to-end workflow for managing overdue invoices. It tracks invoices, due dates, and days overdue. It assigns urgency levels, for example Friendly, Firm, or Final. It generates collection emails using an AI model, Gemini 3.5, with a well-defined JSON output structure. It sends emails through Resend, or it records them in a local log when no email provider is configured. It includes an ROI calculator that estimates recovered cash and the impact on DSO. It exposes a modern dashboard for finance or AR staff to work with.

The goal is to give a buyer a ready-made foundation for a commercial AR automation product, not just a simple demo or prototype.

## 2. Architecture

### Frontend

The frontend is a React application built with Vite. It uses Tailwind CSS for layout and styling. The main interface components include an aging arrears ledger that shows invoices and their status, an AI email drafting and preview panel, an ROI calculator for estimating financial impact, and a log view for system and email events.

### Backend

The backend is a Node.js server using Express as the API server. It supports multiple tenants via a tenant_id field and an x-tenant-id HTTP header. It uses a symmetric cryptographic vault based on AES-256-GCM for encrypting sensitive data, such as Resend API keys, email provider credentials, and other configuration values. The database is SQLite in WAL mode, which allows reliable concurrent reads and writes.

### AI and Automation

The system integrates with Gemini 3.5 via the @google/genai SDK. It has an AI drafting endpoint that accepts invoice and debtor information, applies a tone such as Friendly, Firm, or Final, and returns JSON with a subject, body, and an array of recoveryTips. It also provides a fallback mode when no AI key is configured, so the system can still be demonstrated locally.

### Email and External Integrations

The system uses the Resend API for sending production emails. When no email key is available, it logs outgoing messages to the database in sandbox mode, which prevents runtime failures during development. The database schema and logic are prepared for Stripe and QuickBooks integrations, including synchronisation of invoices and customer details, and storage of provider references and URLs.

## 3. Data Model (High-Level)

The database schema is designed around a multi-tenant pattern. At a high level, it includes tenants, which are records for each tenant or company, tenant_settings for configuration per tenant such as company name, support email, API keys, and dunning_templates for Friendly, Firm, and Final messages. It also includes invoices, which are invoice records with amount due, currency, days overdue, current status, and links to online invoice pages, jobs for background job records such as ledger synchronisation tasks, logs as a log of system events including sandboxed email deliveries, and oauth_tokens for storage of access and refresh tokens for external APIs.

Monetary amounts are stored as integers in minor units, for example cents, to avoid floating-point rounding errors.

## 4. Technology Stack

The technology stack includes:

- Frontend: React, Vite, Tailwind CSS
- Backend: Node.js, Express, TypeScript
- Database: SQLite, with WAL mode enabled
- AI: Gemini 3.5 via @google/genai SDK
- Email: Resend, with a local sandbox fallback
- Background processing: Python daemon for ledger parsing and synchronisation, for example with Stripe and QuickBooks

## 5. Runtime Design (Conceptual Flow)

The frontend sends HTTP requests to /api/... endpoints, including an x-tenant-id header to identify the tenant. The Express backend resolves the tenant context and reads from and writes to the SQLite database using tenant-scoped queries. The AI drafting endpoint at /api/draft-ai-email receives invoice and debtor information, calls Gemini 3.5, or uses a safe fallback when no API key is present, and returns JSON containing the email subject, body, and recovery tips. The email sending endpoint at /api/invoices/send-custom sends the email to the debtor if a valid Resend API key exists, and if not, it records a sandbox event in the logs table and marks the invoice as having a reminder sent. A Python daemon runs separately to synchronise external ledger data and update the invoices and jobs tables. The system is intended to be container-friendly and suitable for deployment on common platforms such as Cloud Run or similar services.

## 6. What Is for Sale

The following are included in the sale:

- Full source code, including frontend (React/Vite/Tailwind), backend (Express/TypeScript), SQLite schema and initialisation scripts, and Python daemon for external ledger synchronisation
- AI integration logic with Gemini 3.5 and structured JSON enforcement
- Email integration logic with Resend and a safe sandbox mode
- The ROI calculator and the main analytics and logging views
- Deployment and configuration instructions sufficient to run the system in a production-like environment

This repository is only a public, high-level overview. The full implementation is provided to the buyer as part of the transfer.

## 7. Sale Details

The asset is Collection Ghost, an AI-powered AR automation and dunning SaaS. The type of sale is source code and full rights, with an exclusive transfer. The stage is pre-revenue, with no active users. The asking price is 5,000 USD, and the lowest acceptable price is 3,000 USD, which is negotiable.

This asset is suitable for AR automation or fintech startups that want to accelerate product development, for B2B SaaS providers that wish to add an AR or dunning module, and for agencies that build finance or back-office systems for clients and want a reusable AR engine.

## 8. Contact

For enquiries about acquiring this project, please contact:

- Email: radwangamerm@gmail.com
