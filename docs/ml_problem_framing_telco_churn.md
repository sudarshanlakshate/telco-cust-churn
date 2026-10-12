# Machine Learning Project Scoping & Problem Framing Document


## 1. Project Overview & Business Value

**Document Name:** `ml_problem_framing_telco_churn.md`
**Project Name:** Telco Customer Retention Engine
**Objective:** Predict individual customer churn behavior to proactively retain customers through targeted marketing and retention programs.
**Date:** October 12, 2026  
**Author/Team:** Sudarshan Lakshate/aiwithsudlak team  

---

## 2. Business Context & System Objectives
* **Supported Decision:** This model supports the **Customer Retention Program Strategy**. Specifically, it automates the decision of whether to trigger a high-value retention offer, contract discount, or personalized outreach to a customer before their billing cycle ends.
* **End User / Consumer:** 
  * **Primary:** The Marketing and Customer Success teams who design and execute automated email/SMS campaigns or place outbound loyalty retention calls.
  * **Secondary:** Executive leadership reviewing monthly churn metrics and retention campaign ROI via internal BI dashboards.

---

## 3. ML Task Formulation
* **Mathematical Frame:** **Binary Classification** (Supervised Learning).
* **Target & Unit of Analysis:** 
  * **Unit of Analysis:** One row represents an individual customer account mapped via a unique `customerID`.
  * **Target Label:** `Churn` (Binary Variable: `Yes` if the customer left within the last month, `No` if they stayed).
* **Feature Scope:**
  * **Demographic Vectors:** `gender`, `SeniorCitizen`, `Partner`, `Dependents`.
  * **Product & Service Usage:** `PhoneService`, `MultipleLines`, `InternetService`, `OnlineSecurity`, `OnlineBackup`, `DeviceProtection`, `TechSupport`, `StreamingTV`, `StreamingMovies`.
  * **Financial & Contract Metrics:** `tenure` (months active), `Contract` (Month-to-month, One year, Two year), `PaperlessBilling`, `PaymentMethod`, `MonthlyCharges`, `TotalCharges`.

## 4. Failure Mode Analysis (What happens when it is wrong?)
Because this is an active intervention system, errors carry direct business costs:

| Error Type | What it means | Business / User Impact |
| :--- | :--- | :--- |
| **False Positive (Type I)** | Model predicts a customer **will churn**, but they had **no intention of leaving**. | **Financial Waste:** The company offers unnecessary discounts or margin-reducing incentives to an already loyal customer, resulting in lower average revenue per user (ARPU). |
| **False Negative (Type II)** | Model predicts a customer **will stay**, but they **actually churn**. | **Revenue Loss:** The highest risk outcome. The company misses the opportunity to save a high-value customer, resulting in lost lifetime value (LTV) and increased customer acquisition costs (CAC) to replace them. |

### Fallback & Risk Mitigation Protocol
* **Confidence Thresholding:** The model will output a probability score (0.0 to 1.0). Instead of acting blindly on "Yes/No", customers with borderline scores (e.g., 0.4 to 0.6) will be routed to a softer, low-cost engagement layer (e.g., product education newsletters) rather than aggressive margin-cutting discounts.
* **Feature Drifts:** The system will flag sudden spikes in `MonthlyCharges` or specific contract changes to trigger immediate heuristic alerts alongside the statistical ML prediction.
