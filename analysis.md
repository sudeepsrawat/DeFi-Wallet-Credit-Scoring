# 📊 Wallet Credit Score Analysis

This document analyzes the behavior of wallets scored using our rule-based credit scoring model on Aave V2 user transaction data.

---

## 🧮 Score Distribution

We assigned a score between **0 and 1000** to each wallet based on features such as deposits, borrowings, repayments, liquidations, and their derived ratios.

The histogram below visualizes the **distribution of credit scores** across all users:

<img width="860" height="547" alt="image" src="https://github.com/user-attachments/assets/a239a989-40a4-4c4e-82ac-b1d0b528a1e0" />


---

## 📉 Score Range Breakdown

| Score Range | Number of Wallets | Description |
|-------------|-------------------|-------------|
| **0–100**   | Very Few          | Extremely risky, likely liquidated or inactive |
| **100–200** | Few               | High liquidation rate, poor repayment |
| **200–300** | Low               | Overleveraged, under-repaid |
| **300–400** | Moderate-Low      | Partial repayments, some redemption, some liquidations |
| **400–500** | Moderate          | Acceptable borrowing, repayment started |
| **500–600** | Healthy           | Responsible users with few risks |
| **600–700** | Safe              | Fully or mostly repaid, conservative borrow-to-deposit ratio |
| **700–800** | Very Safe         | Actively repaid, no liquidation, high redemption |
| **800–900** | Trustworthy       | Strong financial discipline, full cycles completed |
| **900–1000**| Ideal             | Top-tier users with optimal behavior |

---

## 🟥 Low Scoring Wallets (0–400)

### Behavioral Traits:
- High liquidation rate (≥ 20%) or multiple liquidation events
- Borrowed without repayment (repay_to_borrow ratio < 0.5)
- Over-leveraging (borrow_to_deposit ratio > 1.5)
- Low transaction counts (often < 3)
- No redemptions — possible abandoned or exploited usage

### Interpretation:
These wallets likely represent:
- Bots or exploiters
- Users who gamed liquidity but never returned
- High-risk actors that contributed negatively to protocol health

---

## 🟨 Mid-Scoring Wallets (400–700)

### Behavioral Traits:
- Mixed behavior: moderate borrowing with partial repayment
- Some redemption of deposits
- Minimal or no liquidation
- Reasonable borrow-to-deposit ratios

### Interpretation:
These users are likely:
- Average protocol participants
- New or irregular users
- Occasionally under-optimized, but not harmful

---

## 🟩 High Scoring Wallets (700–1000)

### Behavioral Traits:
- Fully repaid borrowed amounts (repay_to_borrow ≥ 1.0)
- No liquidations
- High redemption ratios (≥ 90%)
- Conservative usage of borrowed funds (borrow_to_deposit < 1)
- Multiple deposits and repayment events

### Interpretation:
These wallets reflect:
- Strong DeFi participants
- Possibly experienced users or power users
- Healthy contributors to protocol liquidity and sustainability

---

## ✅ Summary

- The **majority of wallets fall between 400 and 600**, indicating generally safe usage with room for improvement.
- There is a **long tail of highly responsible wallets** that show strong repayment and zero-liquidation behavior.
- Very low-score wallets are rare, but exist — likely from liquidated or abandoned strategies.

---
