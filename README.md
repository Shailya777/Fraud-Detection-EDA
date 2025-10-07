# Fraud Detection Analysis (EDA with Statistical Validation on Synthethic Dataset)

## 📌 Project Overview
This project analyzes fraud detection using Exploratory Data Analysis (EDA) with **statistical hypothesis testing**. 
It goes beyond visual exploration to validate findings using Chi-square tests and t-tests.

## 🛠️ Tools Used
- Python, Pandas, NumPy
- Matplotlib, Seaborn
- SciPy (statistical tests)
- Sklearn (PCA)

## 🔍 EDA Workflow
1. Data Cleaning & Exploration
2. Univariate Analysis (distributions)
3. Bivariate Analysis (numerical vs categorical vs target)
4. Multivariate Analysis (correlations)
5. Statistical Tests (Chi-square, T-test, ANOVA)
6. Insights & Recommendations

## 📊 Key Statistical Findings
- Chi-Squared Contingency Test:
    - According to Chi-Square Contingency Test Results Above, Fraud has No Significant Association with Any of the Categorical Features.
  
- T Test:
    -  According to t-test, There's no significant difference between Numerical Features in Fraud and Non-Fraud Transactions.

## ✅ Business Recommendations
- Fraud is a "Needle in a Haystack" Problem: With fraudulent transactions making up less than 5% of the data, the core business challenge is to accurately identify these rare
   events without disrupting the vast majority of legitimate customer transactions. Overly aggressive fraud detection could lead to high rates of false positives,
   harming customer experience.
- The analysis shows that fraudulent activities aren't tied to obvious, isolated factors. For instance, fraud isn't significantly more common on a specific device,
  at a certain time of day, or in a particular merchant category. This means a simple rule-based system will be ineffective.
- While no single feature predicts fraud, certain combinations of behaviors serve as strong red flags. The most actionable insights come from combining features, such as:
    - New Accounts with High-Value Transactions: A recently created account making a large purchase is a classic high-risk pattern.
    - Sudden Changes in Spending: A user whose transaction amount suddenly spikes far above their historical average could indicate a compromised account.

*The remarkably uniform distribution of transactions across hours, weekdays, and months is unusual for real-world financial data. 
This suggests the dataset might be synthetic or heavily sampled. A model trained on this data must be carefully validated on real transactional data before being deployed, 
as it may not capture typical real-world patterns like dips in activity overnight or spikes during holidays.*


## ✅ Conclusion
The exploratory data analysis reveals that fraud detection in this dataset is a complex challenge that cannot be solved with simple, linear models or straightforward rules. The key finding is that no single feature, whether numerical or categorical, has a strong, direct relationship with fraud. This was consistently demonstrated through multiple analyses:
- Bivariate analysis showed weak to no correlation between numerical features and fraud.
- Statistical tests (Chi-Square and t-test) confirmed this, finding no significant statistical association between any individual feature and the fraud label.
- Principal Component Analysis (PCA) showed that the data's variance is spread evenly across multiple components, meaning there is no single, dominant pattern to exploit.

While individual features are weak predictors, the EDA successfully identified several behavioral patterns (e.g., new accounts making large purchases) that are indicative of higher risk.

---

## 🚀 How to Run This Project
1. Clone this repository or download the files.
2. Install dependencies (preferably in a virtual environment):

   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn scipy
   ```

3. Open the notebook:

   ```bash
   jupyter notebook Fraud Detection.ipynb
   ```

4. Run cells step by step to reproduce the analysis and plots.

---

## 📌 Author
- Prepared by **Shailya Gandhi**(https://www.linkedin.com/in/shailya-gandhi-b395a953/)
- This project is intended as a professional portfolio piece showcasing **EDA skills for Data Analytics / Data Science** roles.

---
