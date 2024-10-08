# Distress in financial institutions during the 2008 housing crisis

This project aims a explaining quantitatively the causes of the distress that many companies that participated in the housing bubble went through between 2007 and 2008.

# Dataset

Details about the dataset to be added.
Some infos can be found [here](https://github.com/Gabriel-dLN/Project/blob/main/data_to_prepare/WRDS_Industry_Financial_Ratio_Manual.pdf).

The dataset contains company-specific variables as well as macroeconomic variables.

Covariates with a 'd' in front of their name (like dinv_turn instead of inv_turn) are the percentage change in these covariates from one quarter to the other.

All the covariates are normalized company-wise.

# Model

The model used throughout this project is a **Time Dependant Cox Proportional Hazard Model**.

For the moment, more on that [here](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/1471-2288-10-20).

# Approach

### Potential biases

# First results

The project is still in a research stage, but in the lab.ipynb can be seen some interesting first results.

In the example below, a Time Dependant Cox Proportional Hazard Model is fitted on the data with the following covariates:
- inv_turn: Inventory Turnover: COGS as a fraction of the average Inventories based on the most recent two periods
- rect_turn: Receivables Turnover: Sales as a fraction of the average of Accounts Receivables based on the most recent two periods
- ddebt_assets: **Variation of** Total Debt/Total Assets: Variation of the Total Debt as a fraction of Total Assets
- dinv_turn: **Variation of** Inventory Turnover: **Variation of** COGS as a fraction of the average Inventories based on the most recent two periods.


From the results below, we can interpret the results:

Note that hazard ratios only make sense when compared across companies. They have no fundamental meaning alone.



![illustration](results/example_res.png "Illustration")

### Covariate Interpretations

#### 1. **inv_turn (Inventory Turnover)**
   - **Coefficient** = 0.43
   - **Hazard Ratio (exp(coef))** = 1.54
   - **Interpretation**: An increase in inventory turnover is associated with a 54% increase in the risk of distress. This could suggest that companies with higher inventory turnover might face operational or financial challenges that could push them toward distress, potentially due to high demand pressures or inefficient management of inventory costs like overnight repos. It could also indicate companies that are struggling to keep up with high market volatility. This totally makes sense: for example a lot of the companies that went bankrupt needed 

#### 2. **rect_turn (Receivables Turnover)**
   - **Coefficient** = -1.03
   - **Hazard Ratio (exp(coef))** = 0.36
   - **Interpretation**: A higher receivables turnover (companies are collecting their receivables more quickly) is associated with a **64% decrease** in the risk of distress. This ratio illustrates the exposure to other financial institutions. It makes sense as companies that are efficient in collecting debts owed to them are less likely to face liquidity issues, thus lowering bankruptcy risk.

#### 3. **ddeb_assets (Variation in Debt to Assets Ratio)**
   - **Coefficient** = 0.38
   - **Hazard Ratio (exp(coef))** = 1.47
   - **Interpretation**: A increase in the debt-to-assets ratio is associated with a **47% increase** in the risk of distress. This aligns with typical financial intuition—companies with higher debt relative to their assets are more likely to face financial strain, especially during downturns.

#### 4. **dinv_turn (Variation in Inventory Turnover)**
   - **Coefficient** = 0.38
   - **Hazard Ratio (exp(coef))** = 1.46
   - **Interpretation**: Higher variations in inventory turnover (the number of days it takes to turn over inventory) increases the risk of distress by **46%**. In fact, companies with a lot of short term debt are more vulnerable during liquidity crisis.

### Multicollinearity Consideration
The correlation matrix at the bottom shows relatively low correlations between the covariates (except **inv_turn** and **dinv_turn** which have a moderate correlation of 0.36). This suggests that multicollinearity is not a significant issue in your model, meaning the estimated effects of each covariate are likely reliable.

### Concordance Index
The concordance index is 0.40, which suggests the model has limited predictive ability. While the coefficients are statistically significant and align with reasonable interpretations, the overall model’s ability to correctly rank companies by their risk of distress is low.

This isn't much of a problem as the model has no predictive purpose. Having a low concordance index but statistically significant variables is still relevant: the model shows that the 4 covariates have a real impact on the hazard ratio and so, the probability to be in distress. For instance, the model reveals that an increase in the inventory turnover and debt to asset ratio, and a decreace in the receivable turnover ratio lead to a higher risk for companies to be in distress.

