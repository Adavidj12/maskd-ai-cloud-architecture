# MASKD — Production Cloud Infrastructure & Architecture Case Study ☁️

**Live Production Application:** 🌐 [maskd-ai.vercel.app](https://maskd-ai.vercel.app)  
*Architected and Integrated by Akpojotor David John*

---

## 📌 Project Overview & Engineering Scope
**MASKD** is a production-grade, real-time AI avatar transformation Software-as-a-Service (SaaS) platform built for creators, live streamers, and VTubers. 

As the **Systems Integrator and Infrastructure Engineer**, my core focus was deploying the full-stack multi-cloud pipeline, securing identity boundaries, implementing robust database access controls, configuring cryptographic payment layers, and building backend fault-tolerance architectures.

> 🔒 **Security Note:** *The core frontend and backend repository source code is maintained in a private repository to protect production database schemas, proprietary backend webhook logic, and API route architectures.*

---

## 🏗️ Production Tech Stack & Multi-Cloud Deployment

*   **Frontend Environment:** React + Vite SPA optimized and globally distributed via **Vercel Edge Hosting**.
*   **Backend Application Layer:** Node.js + Express API services containerized and deployed on **Render Web Services**.
*   **Database Management (BaaS):** PostgreSQL infrastructure provisioned, structured, and scaled via **Supabase**.
*   **Identity & Access Management (IAM):** Secure cross-platform authentication managed via **Clerk**.
*   **Decentralized Payment Gateway:** Native, direct-to-wallet cryptocurrency checkout verified via **Blockonomics API**.
*   **Real-Time AI Video Streaming:** High-performance stream diffusion pipelines powered via **Decart Lucy 2.1 API**.

---

## ⚙️ Core Infrastructure Achievements & Systems Engineering

### 1. Identity Management & Network Security
*   **Cross-Platform JWT Verification:** Enforced secure Clerk frontend authentication and mirrored it with rigorous backend JSON Web Token (JWT) verification to prevent unauthorized API requests.
*   **Supabase Row Level Security (RLS):** Enabled and configured strict RLS policies on PostgreSQL tables, ensuring users can only read/write data assigned to their UUID, protecting sensitive admin data.
*   **Rate Limiting & Protection:** Deployed backend request rate-limiting layers to mitigate Distributed Denial of Service (DDoS) attempts and programmatic API abuse.

### 2. High-Availability & High-Performance AI Orchestration
*   **AI Provider Abstraction & Failover:** Configured an automated infrastructure fallback layer (`AI_PROVIDER_FALLBACKS`). If the primary Decart Lucy 2.1 API hits a cooldown error, the backend dynamically redirects traffic to a fallback provider to eliminate service downtime.
*   **Real-Time Session Billing Synchronisation:** Engineered a real-time event logging system tracking `ai_sessions` and `product_events` to compute usage-based crypto credit deductions instantly.
*   **Infrastructure Resiliency (UptimeRobot):** Integrated persistent synthetic network pings to the Supabase database to circumvent cold starts, ensuring immediate responsiveness for users entering the stream studio.

### 3. Payment Gateway & Webhook Idempotency
*   **Direct-to-Wallet Transactions:** Integrated **Blockonomics** to process crypto transactions directly to native wallets without relying on vulnerable third-party payment processors.
*   **Idempotency Locking:** Implemented a secure backend webhook listener that checks for duplicate transactions to prevent race conditions or double-crediting user profiles during rapid payment clicks.

---

## 📊 Database Schema & Administrative Controls

The backend maps relationships across nine production tables designed for auditable, structured enterprise data:
- `users` & `admin_users` (Identity Isolation)
- `purchases` & `ai_sessions` (Financial & Compute Logging)
- `admin_audit_logs` & `product_events` (Security & Analytics Auditing)
- `user_avatar_state`, `support_tickets`, & `user_terms_acceptance` (State Management)

### Operational Admin Tooling
Architected a secure administrative dashboard interface allowing IT support personnel to seamlessly investigate account anomalies:
*   Search database users and execute real-time credit adjustments (Add/Remove credits).
*   Force-stop active rogue server sessions to conserve compute resources.
*   Monitor active AI pipeline metrics and view cryptographically signed system audit logs.

---

## 🔒 Configuration & Environment Variable Framework
To protect production integrity, environment variables are completely decoupled from the codebase and injected securely through Vercel and Render management dashboards:

*   **Security & Identity Keys:** `CLERK_SECRET_KEY`, `SUPABASE_SERVICE_ROLE_KEY`
*   **Orchestration Configurations:** `AI_PROVIDER_FALLBACKS`, `PROVIDER_FAILURE_COOLDOWN_MS`
*   **Payment & Webhook Verification:** `BLOCKONOMICS_CALLBACK_SECRET`, `DECART_API_KEY`

---

## 🛠️ Production Troubleshooting & Engineering Interventions

During the deployment of MASKD across a distributed architecture, I isolated and resolved critical infrastructure errors to stabilize the production pipeline:

### 1. Cross-Origin Resource Sharing (CORS) Blocks
*   **The Issue:** The React client hosted on Vercel (`maskd-ai.vercel.app`) was blocked from communicating with the Express API server deployed on Render. Web browsers blocked incoming stream data payloads due to missing cross-origin permission boundaries.
*   **The Resolution:** I modified the Node.js backend configuration to utilize the `cors` middleware dynamically. I explicitly whitelisted the production Vercel frontend URL within the `FRONTEND_ORIGIN` environment array, configured specific allowed methods (`GET, POST, OPTIONS`), and enabled `credentials: true` to pass secure Clerk authentication tokens safely across domains.

### 2. Runtime Environment Variable Validation Failures
*   **The Issue:** Initial backend container deployments on Render crashed immediately with undefined reference errors because critical cryptographic keys (`CLERK_SECRET_KEY`, `DECART_API_KEY`) were not being injected before the startup scripts initialized.
*   **The Resolution:** I restructured the backend startup sequence by creating an initial validation script using environment checking layers. I synchronized the deployment dashboards across Render and Vercel, ensuring that every secret listed in the architecture requirements was manually bound to the production server runtime environment rather than hardcoded into source repositories.

### 3. Database Connection & Cold-Start Delays
*   **The Issue:** Due to inactivity periods during initial beta testing, the serverless Supabase PostgreSQL database would enter hibernation mode. This caused subsequent user requests to time out while waiting for the database to wake up.
*   **The Resolution:** I integrated **UptimeRobot** to act as an automated synthetic monitoring tool. I configured it to run lightweight, automated HTTP ping traffic directly to the server every 5 minutes. This keep-alive configuration maintains the connection line constantly, guaranteeing immediate database response times for active users entering the stream studio.
