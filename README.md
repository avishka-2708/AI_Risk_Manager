# AI_Risk_Manager
# 🛡️ Autonomous Risk & Chargeback Triage Agent
> **Razorpay AI Buildathon Submission — Track 02: AI Risk Manager**

An agentic defense engine that evaluates incoming transaction streams, predicts anomaly risk, and dynamically executes risk-mitigation actions by weighing financial chargeback exposure against false-positive customer friction costs.

---

## 📌 Executive Summary

Fraudulent transactions and chargebacks quietly erode merchant margins[cite: 1]. Traditional rule-based engines either let complex fraud slip through or block legitimate users, causing high friction costs. 

This project implements a **Hybrid ML + Agentic Framework**:
1. **Unsupervised Anomaly Detection:** An Isolation Forest model trained on transaction parameters (velocity, payment amounts, IP distance, and failure history).
2. **Friction-Aware Decision Tools:** LangChain tools that calculate net financial exposure and evaluate false-positive friction penalty vs. chargeback risk.
3. **Multi-Tiered Action Protocol:** Dynamically triggers `APPROVE`, `STEP_UP_AUTH` (2FA verification), or `FREEZE_PAYOUT` with structured audit trails.

---

## 📊 Performance Metrics (Held-Out Test Set)

Evaluated on 1,000 held-out test transactions[cite: 1]:

| Metric | Score / Value | Description |
| :--- | :--- | :--- |
| **Recall** | **100.00%** | **0 missed fraud cases** (Zero chargeback leakage) |
| **Precision** | **73.08%** | Optimized to minimize false-positive friction|
| **F1-Score** | **0.8444** | Balanced metric for risk performance |
| **Total Fraud Prevented** | **₹266,568.64** | 100% defense efficiency on test dataset |
| **Missed Fraud Loss (FN)**| **₹0.00** | Complete capture of high-risk transactions|

### Action Breakdown
* **`APPROVE` (948 transactions):** Cleared low-risk orders automatically.
* **`STEP_UP_AUTH` (35 transactions):** Challenged medium-risk orders with secondary auth (2FA) to avoid outright blocking legitimate buyers.
* **`FREEZE_PAYOUT` (17 transactions):** Intercepted high-risk, high-value fraud threats immediately.

---

## 🏗️ Architecture & Workflow

```text
                               ┌──────────────────────────┐
                               │     Incoming Payload     │
                               └────────────┬─────────────┘
                                            │
                                            ▼
                               ┌──────────────────────────┐
                               │ Isolation Forest Engine  │
                               └────────────┬─────────────┘
                                            │
                                            ▼
                               ┌──────────────────────────┐
                               │  Anomaly & Risk Scoring  │
                               └────────────┬─────────────┘
                                            │
                                            ▼
                               ┌──────────────────────────┐
                               │  Financial Friction &    │
                               │  Risk Exposure Tool      │
                               └────────────┬─────────────┘
                                            │
               ┌────────────────────────────┼────────────────────────────┐
               ▼                            ▼                            ▼
     ┌───────────────────┐        ┌───────────────────┐        ┌───────────────────┐
     │      APPROVE      │        │   STEP_UP_AUTH    │        │   FREEZE_PAYOUT   │
     │  (Low Risk /      │        │  (Medium Risk /   │        │  (High Risk /     │
     │   Clean Tx)       │        │   Require 2FA)    │        │   Freeze Funds)   │
     └─────────┬─────────┘        └─────────┬─────────┘        └─────────┬─────────┘
               │                            │                            │
               └────────────────────────────┼────────────────────────────┘
                                            │
                                            ▼
                               ┌──────────────────────────┐
                               │  Structured JSON Audit   │
                               │         Trail            │
                               └──────────────────────────┘
## 🛠️ Repository Structure

```text
├── Razorpay_AI_Risk_Manager.ipynb 
├── app.py                         
├── requirements.txt                
└── README.md
