# Model Card: Loan Delinquency Classifier (Decision Tree + EBM)

- **Author**: Ari Bernstein  
- **Contact**: [GitHub Repository](https://github.com/aribernstein1/assignments)  
- **Date**: May 2025  
- **Course**: DNSC 6330 Responsible Machine Learning  
- **Best Model**: Explainable Boosting Machine (EBM)


## 1. Intended Use
**Describe the business value of your group’s best remediated model:**
The model helps financial institutions assess the risk of loan delinquency, enabling more informed decisions in credit management and reducing losses from defaults.

**Describe how your group’s best remediated model is designed to be used:**
It is designed as a decision-support tool to flag high-risk borrowers based on past behavior and financial patterns, ideally used alongside human review.

**Describe the intended users for your group’s best remediated model:**
Risk analysts, loan officers, compliance auditors, and credit managers in banks or financial services organizations.

**State whether your group’s best remediated model can or cannot be used for any additional purposes:**
The model is not intended for use in determining loan interest rates, credit limits, or eligibility without further fairness testing or regulatory oversight.

## 2. Training Data
- Source: GWU Blackboard
- Training size: 15,000 rows
- Validation size: 7,500 rows
- Test size: 7,500 rows
- Features: PAY_0–6, BILL_AMT1–6, PAY_AMT1–6, LIMIT_BAL, SEX, RACE, etc.
- Target: DELINQ_NEXT (1 = late, 0 = on-time)

### Feature Correlation Heatmap:
![Unknown](https://github.com/user-attachments/assets/793fbe88-09ce-44dc-be29-de42b22b148a)


### Feature Distributions:
![Unknown-2](https://github.com/user-attachments/assets/dcd6420c-e4e5-4455-bc54-2a34f762c855)


## 3. Evaluation Data
- Same schema as training
- Test set size: 7,500 rows
- Held out until final evaluation

## 4. Model Details
- Type: Decision Tree initially, then remediated using EBM (Explainable Boosting Machine)
- Software: Python, scikit-learn, interpretML
- Best EBM Hyperparameters: defaults with 16 bins per feature and 3000 interactions

### EBM Feature Importance: 
![Unknown-7](https://github.com/user-attachments/assets/f7d8a9b8-c584-40e5-953f-c96870504a9d)


### EBM Partial Dependence Plot: 
![Unknown-8](https://github.com/user-attachments/assets/ed812b8a-ea2e-47fd-9ed9-54e9a78a3400)


## 5. Quantitative & Fairness Analysis
- Validation AUC: 0.7891
- Test AUC: 0.7687
- Validation AIR values:
  - Black vs. White: 0.8345
  - Hispanic vs. White: 0.8765
  - Asian vs. White: 1.098
  - Female vs. Male: 1.245

### AIR vs AUC Tradeoff:
![Unknown-9](https://github.com/user-attachments/assets/10975ad9-7656-43ac-9dba-b42d4e0ad234)



### Confusion Matrices by Subgroup:
<img width="304" alt="Screenshot 2025-05-05 at 6 32 56 PM" src="https://github.com/user-attachments/assets/8ab8126f-4cc3-42e4-9caf-4c5b83a1dc93" />


## 6. Ethical Considerations
**Describe potential negative impacts of using your group’s best remediated model:**

*Math or software problems:*
The model may overfit to training data distributions and underperform on unseen subpopulations.
Software issues such as misapplied normalization or incorrect feature encoding (e.g., with demographic variables) could bias predictions.
*Real-world risks – who, what, when, how:*

*Who*: Borrowers flagged as high risk by the model.

*What*: May face reduced credit limits, higher interest rates, or loan rejections.

*When*: During loan approvals, renewals, or credit scoring processes.

*How*: Institutions using the model without human review or transparency could apply adverse actions unjustly.


**Describe potential uncertainties relating to the impacts of using your group’s best remediated model:**

*Math or software problems:*
Metrics like AUC and AIR don’t capture all dimensions of fairness (e.g., precision/recall by subgroup).
Uncertainty remains regarding how robust the model is across different geographic or socioeconomic data slices.


*Real-world risks – who, what, when, how:*

*Who*: Consumers in borderline cases or minority groups.

*What*: May receive unfair risk assessments due to unmodeled bias.

*When*: If the data pipeline or distribution shifts after deployment.

*How*: Through automated decision pipelines that lack transparency or appeal processes.

**Describe any unexpected or results encountered during training:**

EBM yielded unexpectedly high AIR values for Female vs. Male groups (>1.2), suggesting the need for further scrutiny.
PAY_AMT variables dominated model importance, even though their direct link to future delinquency may be context-dependent.
GLM and MXGB had weaker fairness tradeoffs, despite similar AUCs, underscoring the need to prioritize interpretable models.

## 7. References
- Hall, P. (2024). DNSC 6330 Lecture Materials
- OpenAI. (2025). ChatGPT (May 3 version) [Large language model]. https://chat.openai.com
