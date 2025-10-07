# Fraud Detection EDA — Insights

This file contains **detailed insights** gathered from the exploratory data analysis (EDA) process.  
It complements the `Fraud Detection.ipynb` notebook by summarizing findings in plain language.

---

## 1. Data Overview
- Dataset contains both **numerical** and **categorical** features.
- A few columns contained Missing Values, whcih were handled using appropriate measures found best suitable for feature. 
- No duplicate records were found.

---

## 2. Data Cleaning
- Transaction Amount Feature found to have missing values, which were handled using Average Transaction Amount -> GRoup median Considering Merchant Category and Device Type -> Global Median.\
- Devicew Type Feature also hasd missing values, which were handled using User Level Mode -> Merchant Level Mode -> Global Mode.
- Merchan Category Feature had missing values, which were handled using Mode of Merchant Category based on Grouping by Location and Device Type -> Mode based on Location -> Random Imputation.
- Account Age Days feature had missing values, filled with Median Based on Customer -> Number of Previous Transaction Based Median -> Global Median.
- Falied Login Attempts Feature had missing values, handled using Random Imputation considering the uniform distribution of the feature.
- Some new features like Year, Month, Day, Hour, Weekday were created using Transaction Time Feature.

**Takeaway:** Clean data ensures reliable insights and avoids skewed analysis.

---

## 3. Univariate Analysis

### Numerical Features
1) Transaction Amount:
   - Distribution is heavily right-skewed, meaning most transactions are of low amounts, with a few very high-value outliers.
   - The boxplot shows multiple extreme outliers, suggesting rare but very large payments.

Insight: High transaction amounts that fall far outside the typical range may indicate:
- Potential fraud attempts (e.g., large unauthorized purchases).
- Or legitimate premium transactions by a small group of users.
- These should be flagged for anomaly detection or threshold-based monitoring.

2) Account Age Days:
   - The distribution is nearly uniform — accounts are evenly spread from newly created to long-standing ones.
   - No visible outliers → dataset includes a balanced age profile.

Insight: Newer accounts (low account_age_days) are often riskier — fraudsters frequently create new accounts for quick misuse.
- Combine with transaction activity:
- New account + high transaction_amount = red flag.
- Old account + stable transaction patterns = lower risk.

3) Number of Previous Transactions:
- Fairly uniform distribution, with customers having a wide variety of previous transaction counts.
- No outliers — transaction histories are consistent.

Insight:
- Low previous transactions + high new transaction amount → potential fraud (inexperienced or fake account making big purchase).
- High previous transactions + stable behavior → likely legitimate user.

4) Average Transaction Amount:
    - Distribution is almost uniform, indicating diversity in spending habits.
    - No major outliers — average spend per user varies broadly but reasonably.

Insight: Sudden spike in a user’s average transaction amount compared to their past average can be a behavioral anomaly.

### Categorical Features
1) Fraud:
   - Fraud Data is Highly Imbalanced.
   - Non-Fraudulent Transactions: 7,604, Fraudulent Transactions (1): 396
   - This means only about 4.95% of the transactions in this dataset are fraudulent. This is a highly imbalanced dataset, which is a crucial consideration for any fraud detection modeling.


2) Transaction Type:
    - Online Purchases (3,262) are the most dominant transaction type, followed by POS Purchases (2,399). Transfers (795) are the least common.


3) Device Type:
    - Transactions are most frequently made on Mobile devices (3,940), with Desktops (3,232) being a close second. Tablets (828) are used far less often.


4) Location:
    - The majority of transactions originate from Urban areas (4,836), more than double the amount from Suburban areas (2,296). Rural transactions (768) are the least frequent.


5) Merchant Category:
    - The distribution across different merchant categories (Travel, Entertainment, Food, Electronics, Clothing) is surprisingly uniform and balanced, with each category having roughly 1,500-1,750 transactions. There is no single dominant spending category.


6) Card Present:
    - Most transactions are card-not-present (5,562) versus card-present (2,438). This aligns perfectly with "Online Purchase" being the most common transaction type.


7) International & High-Risk Country:
    - The dataset is heavily skewed towards domestic transactions. The vast majority of transactions are not international (is_international= 7,172) and not from a high-risk country (is_high_risk_country= 7,800). The rare instances where these are true could be strong indicators of fraud.


8) Failed Login Attempts:
    - The number of failed login attempts is distributed relatively evenly across 0, 1, 2, and 4 attempts. Unlike other risk features, there isn't a strong skew, suggesting that multiple login attempts are common in this dataset.


9) Year:
    - All data is from a single year, 2023.


10) Month:
    - The transaction count is very consistent across most months (around 700-750). The slight dip in February (month 2) is expected due to it having fewer days.


11) Day:
    - There's a clear and logical pattern here. Days 1 through 28 have a consistent count (264). The count then drops for days 29, 30, and 31, which correctly reflects that not all months have these days.


12) Hour & Weekday:
    - Both of these distributions are remarkably uniform. There are no obvious peaks or troughs for time of day or day of the week. This is unusual for financial data, which typically shows spikes during business hours or weekends. This uniformity might suggest the data is synthetic, has been sampled evenly, or represents a global user base operating across all time zones.

---

## 4. Bivariate Analysis

### Numerical vs Numerical
- The heatmap shows that there are no strong correlations (positive or negative) among the numerical features.
- All pairwise correlations between different features (such as transaction_amount vs. num_prev_transactions) are close to zero, indicating negligible linear relationships between them.
- The pairplot visually confirms the weak or absent relationships.
- There is no indication of multicollinearity or redundant information among the selected features.
- Bivariate relationships are weak, meaning each feature may contribute independently in modeling scenarios, without risk of double-counting similar information.

In summary, the numerical features in the dataset are largely independent of each other, with minimal bivariate correlation or interaction present.

### Categorical vs Numerical
- transaction_amount displays notable differences across categories for transaction type (Online Purchase, ATM Withdrawal, POS Purchase, Transfer); Online Purchases and Transfers have more outliers towards higher values, while ATM Withdrawals are tightly clustered at lower amounts.

- Several categorical features, like device type, location, and merchant category, show that transaction amounts have a skewed distribution with many outliers, but the medians are globally similar, implying no major differences by those categories.

- The distributions of account_age_days, num_prev_transactions, and avg_transaction_amount are very uniform across most categories; medians and quartiles are very similar, with no distinct separation by device, location, merchant, risk category, fraud report, or year.

- For categorical features like device type, location, category, and year, there is no visually clear pattern or separation in numerical feature distributions, suggesting independence of these features or no major impact from category on age, count, or average transaction value.

- Transaction amount has visible outliers for most categorical values (such as certain merchant categories and months); this indicates that some subgroups experience sporadic high-value transactions, but these do not shift the central tendency.

- Median and spread for all numerical features appear virtually identical across binary categories like fraud report (yes/no) and risk category (0/1), suggesting these categorical targets do not have a strong linear influence on any numerical feature in this dataset.

- By year and by month, the medians and quartiles for all numerical features are consistent, implying no seasonality or year-based trend detectable from visual inspection.

- Overall Summary:
    - The categorical features have minimal observable impact on the distribution of the numerical features, except for some transaction types, which show differences in spread and outliers of transaction_amount.
    - Most categorical-numerical pairings suggest independence. Thus, feature engineering or deeper statistical tests may be needed to detect subtle patterns.

---

## 5. Multivariate Analysis (PCA)
- The explained variance ratio for the first four principal components is quite balanced, with each contributing approximately 25% to the total variance in the data. This indicates there’s no single dimension that overwhelmingly captures the variance; instead, the information is distributed across components evenly.
- The scatter plots between any two principal components (PC1, PC2, PC3, PC4) reveal that the data points form roughly circular clusters. This pattern suggests little to no strong linear correlation remaining between principal components.
- These findings confirm that the PCA has produced uncorrelated and informative new features, but does not reveal hidden groupings or significant redundancy in the original data.

---

## 6. Statistical Tests:
- According to Chi-Square Contingency Test Results Above, Fraud has No Significant Association with Any of the Categorical Features.

- According to t-test, There's no significant difference between Numerical Features in Fraud and Non-Fraud Transactions.

---

## 7. Overall Business Insights
- Fraud is a "Needle in a Haystack" Problem: With fraudulent transactions making up less than 5% of the data, the core business challenge is to accurately identify these rare events without disrupting the vast majority of legitimate customer transactions. Overly aggressive fraud detection could lead to high rates of false positives, harming customer experience.
- The analysis shows that fraudulent activities aren't tied to obvious, isolated factors. For instance, fraud isn't significantly more common on a specific device, at a certain time of day, or in a particular merchant category. This means a simple rule-based system will be ineffective.
- While no single feature predicts fraud, certain combinations of behaviors serve as strong red flags. The most actionable insights come from combining features, such as:
    - New Accounts with High-Value Transactions: A recently created account making a large purchase is a classic high-risk pattern.
    - Sudden Changes in Spending: A user whose transaction amount suddenly spikes far above their historical average could indicate a compromised account.

*The remarkably uniform distribution of transactions across hours, weekdays, and months is unusual for real-world financial data. This suggests the dataset might be synthetic or heavily sampled. A model trained on this data must be carefully validated on real transactional data before being deployed, as it may not capture typical real-world patterns like dips in activity overnight or spikes during holidays.*

---

## ✅ Conclusion

The exploratory data analysis reveals that fraud detection in this dataset is a complex challenge that cannot be solved with simple, linear models or straightforward rules. The key finding is that no single feature, whether numerical or categorical, has a strong, direct relationship with fraud. This was consistently demonstrated through multiple analyses:
- Bivariate analysis showed weak to no correlation between numerical features and fraud.
- Statistical tests (Chi-Square and t-test) confirmed this, finding no significant statistical association between any individual feature and the fraud label.
- Principal Component Analysis (PCA) showed that the data's variance is spread evenly across multiple components, meaning there is no single, dominant pattern to exploit.

While individual features are weak predictors, the EDA successfully identified several behavioral patterns (e.g., new accounts making large purchases) that are indicative of higher risk.

---