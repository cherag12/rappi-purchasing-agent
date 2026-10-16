# Rappi BuyAgent — Autonomous AI Purchasing Agent
> First Round Selection Assignment for Rappi India (Moustache Technology)

An enterprise autonomous purchasing agent designed for quick-commerce buyers (Rappi Turbo). It continuously investigates dark-store inventory telemetry, executes multi-factor quantitative decisions, generates ERP purchase orders, and validates fulfillment through autonomous 3-way reconciliation.

---

## 🎯 Core Purchasing Pipeline (The 4 Stages)

1. **🔍 Investigate**:
   - Queries real-time dark-store inventory on-hand vs. safety stock thresholds.
   - Computes hourly sales velocity ($\text{units/hour}$) and stockout horizons ($4.9\text{ hrs}$ runway).
   - Scans supplier catalog directories for unit pricing, transport distance, MOQs, and SLA ratings.

2. **🧠 Decide**:
   - Solves multi-factor trade-offs: penalizes suppliers whose lead time breaches the stockout window ($6.0\text{h} > 4.9\text{h} \rightarrow \text{lost GMV}$).
   - Determines Economic Reorder Quantity (EOQ) covering peak hours while satisfying MOQs.
   - Generates executive rationale powered by Gemini 3.8 Flash.

3. **⚡ Take Action**:
   - Dispatches official ERP Purchase Orders (e.g. `PO-RAPPI-2026-9042`).
   - Simulates vendor EDI 850 / REST API dispatch.
   - Enforces Human-in-the-Loop approval limits (configurable from $1,000 to $10,000).

4. **✅ Validate Outcome**:
   - Automated 3-way reconciliation: Purchase Order $\leftrightarrow$ Advance Shipping Notice (ASN) $\leftrightarrow$ Physical Dock Scan $\leftrightarrow$ Commercial Invoice.
   - Audits cold-chain IoT temperature logs ($3.8^\circ\text{C}$ within safe range).
   - Confirms extended inventory safety runway.

---

## 🏗️ Tech Stack

- **Frontend**: React 19, TypeScript, Tailwind CSS, Lucide Icons
- **Full-Stack Runtime**: Express.js server + Vite middleware (`server.ts`)
- **AI Engine**: `@google/genai` TypeScript SDK (`gemini-3.8-flash`)
- **Backend Architecture Included**: Python 3.11+, Django 5.x, Django REST Framework, Celery, Redis, Pytest

---

## 🚀 Quick Start (Local Setup)

### 1. Install Dependencies
```bash
npm install
```

### 2. Environment Variables
Create a `.env` file from `.env.example`:
```bash
cp .env.example .env
```
*(Optional: add `GEMINI_API_KEY="your_api_key"` for live AI reasoning)*

### 3. Start Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🐍 Python Django Backend Code

The repository includes complete Django implementation files in `src/data/djangoCodebase.ts` and viewable via the **Django Architecture** tab:
- `purchasing_agent/models.py` (Relational schemas for SKUs, MFC hubs, and 3-way audits)
- `purchasing_agent/agent/buyer_agent.py` (Python ReAct purchasing agent with tools)
- `purchasing_agent/views.py` (DRF API ViewSets)
- `purchasing_agent/tasks.py` (Celery 5-minute periodic stockout scanner)
- `purchasing_agent/tests/test_agent.py` (Pytest end-to-end verification suite)
