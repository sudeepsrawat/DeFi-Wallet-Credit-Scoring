# 🧮 DeFi Wallet Credit Scoring — Aave V2 Transaction Analysis

This project assigns a **credit score (0–1000)** to each wallet address interacting with the Aave V2 DeFi protocol. The score reflects the user's trustworthiness based on historical transaction behavior.

---

## 📁 Dataset

**Source**: Sample of ~100K user-level transactions from Aave V2 protocol.  
**Format**: JSON file (`user-wallet-transactions.json`)

Each record includes:
- `userWallet`: Wallet address
- `timestamp`: Unix timestamp
- `action`: One of `deposit`, `borrow`, `repay`, `redeemunderlying`, or `liquidationcall`
- `actionData`: Includes `amount`, `assetSymbol`, `assetPriceUSD`

---

## 🧠 Method Chosen: **Rule-Based Scoring Model**

We chose a **rule-based scoring approach** for transparency, interpretability, and due to the absence of ground truth labels for supervised machine learning. The rule engine reflects rational credit behavior in DeFi protocols, rewarding repayment, conservative usage, and penalizing liquidation or leverage abuse.

---

## 🏗️ Architecture Overview

Raw JSON Data
↓
Data Parser (Normalize + Extract)
↓
User-Level Feature Engineering
↓
Rule-Based Scoring Engine
↓
Credit Score (0–1000) per Wallet
↓
Score Visualization + Analysis

---

## 🔄 Processing Flow

### 1. **JSON Parsing & Transformation**
- Convert Unix timestamps to `datetime`
- Extract and normalize `amount` and `price`
- Compute USD value = `amount * assetPriceUSD`

### 2. **Feature Engineering**
Aggregate transactions for each wallet to compute:
- `num_deposit`, `num_borrow`, `num_repay`, `num_redeem`, `num_liquidation`
- USD totals for deposit/borrow/repay/redeem
- Ratios:
  - `borrow_to_deposit_ratio`
  - `repay_to_borrow_ratio`
  - `redeem_to_deposit_ratio`
  - `liquidation_rate`

### 3. **Rule-Based Scoring Engine**
Each wallet starts at **500** and receives:
- **Penalties** for:
  - High liquidation rate / events
  - Over-leveraging (borrow >> deposit)
  - Borrowing without repayment
  - Very low activity

- **Rewards** for:
  - Conservative borrowing
  - Full repayment or over-repayment
  - High redemption
  - Healthy participation (no liquidation, many transactions)

Final score is clipped between **0 and 1000**.

## 📁 Outputs
features_df: Wallet-level features + credit scores

score_distribution.png: Score distribution chart

---

## 📌 Future Extensions
Use supervised learning (XGBoost, LightGBM) if labeled repayment data is available

Cluster wallets using unsupervised ML for bot/exploit detection

Add time-based features (e.g., repayment latency)

---
