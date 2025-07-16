# 🔐 DeFi Wallet Credit Scoring with XGBoost (Aave V2)

This project builds a machine learning-based **credit scoring system** for DeFi wallets interacting with the **Aave V2 protocol**. Using raw transaction-level data (~100K records), we assign a **credit score between 0 and 1000** to each wallet, where higher scores indicate safer and more responsible users.

----------

## 🧠 Problem Statement

You are given raw, transaction-level DeFi data from the Aave V2 protocol.  
Each record captures a user action such as:

-   `deposit`
    
-   `borrow`
    
-   `repay`
    
-   `redeemunderlying`
    
-   `liquidationcall`
    

### 🎯 Goal

Assign a **credit score between 0 and 1000** to each wallet based on its historical DeFi behavior.  
Higher scores indicate **reliable users**, while lower scores flag **risky or exploitative wallets**.

----------

## ✅ Method Chosen

We followed a **two-step hybrid approach**:

1.  **Rule-Based Scoring Logic**
    
    -   Create explainable, behavior-driven pseudo scores.
        
    -   Capture liquidation risk, over-borrowing, and repayment reliability.
        
2.  **Machine Learning with XGBoost**
    
    -   Train an XGBoost classifier using engineered features.
        
    -   Learn score labels derived from the rule-based logic for better generalization.
        

This hybrid approach ensures:

-   **Transparency** via explicit scoring logic
    
-   **Scalability** through a data-driven ML model
    

----------

## ⚙️ Architecture & Flow
```text
Raw JSON
   ↓
Parsed DataFrame
   ↓
Feature Engineering
   ↓
Rule-Based Score (0–1000)
   ↓
Label Binning (5 classes: 0 to 4)
   ↓
XGBoost Model Training
   ↓
Predicted Labels → Scaled to Final Credit Scores
   ↓
Output CSV: Wallet → Score
```


## 🔍 Feature Engineering

Each wallet is represented using these features:

Feature

Description

`num_transactions`

Total number of transactions

`num_borrow`, `num_repay`

Borrow and repay counts

`borrow_to_deposit_ratio`

Indicator of over-borrowing behavior

`repay_to_borrow_ratio`

Measures how responsibly the user repays loans

`redeem_to_deposit_ratio`

Frequency of withdrawal post-deposit

`liquidation_rate`

Fraction of interactions that triggered liquidation

----------

## 🧮 Score Logic (Rule-Based)

Initial score: **500**

### Adjustments:

-   **Liquidation Rate**:
    
    -   > 0.5 → −300
        
    -   > 0.2 → −200
        
    -   > 0 → −100
        
-   **Borrow-to-Deposit Ratio**:
    
    -   > 2 → −150
        
    -   > 1 → −100
        
    -   ≤ 1 → +50
        
-   **Repay-to-Borrow Ratio**:
    
    -   ≥ 1 → +300
        
    -   ≥ 0.75 → +100
        
    -   ≥ 0.5 → +50
        
    -   < 0.5 → −100
        
-   **Redeem-to-Deposit Ratio**:
    
    -   ≥ 0.9 → +50
        

Final score is capped between **0 and 1000**.

----------

## 🧠 ML Model: XGBoost

-   Model Type: `XGBClassifier`
    
-   Input: Engineered features
    
-   Labels: Derived from binning rule-based scores into 5 classes (using `pd.cut()`)
    
-   Output: Wallet label → mapped to score
    

Label

Mapped Credit Score

0

100 (Very Risky)

1

300

2

500 (Average)

3

700

4

900 (Very Safe)

----------

## 📊 Outputs

-   `wallet_scores.csv`: Final predicted scores for each wallet
    
-   `analysis.md`: Score distribution & behavioral trends
