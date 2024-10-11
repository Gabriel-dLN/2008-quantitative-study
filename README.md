# Study of the causes that led financial institutions to bankruptcy during the 2008 housing bubble using a **Time Dependant Cox Proportional Hazard Model**


This project aims at providing a quantitative explaination of the causes that led the companies which participated in the speculation around the 2008 housing bubble to bankruptcy.

It uses a variant of a model commonly used in Survival Analysis which is the Cox Proportional Hazard Model.

# Dataset

The dataset has been created by querying data form the [Center for Research in Security Prices, LLC (CRSP)](https://www.crsp.org/), and the [Federal Reserve Bank of St. Louis](https://fred.stlouisfed.org/) and is composed (at the moment) 37 companies, 19 of which were in financial trouble during the 2008 crisis.

It contains many different financial ratios reported quartely taken form 3 different categories: Efficiency (ex: Inventory Turnover, ...), Financial Soundness (ex: Short-Term Debt/Total Debt ratio, ...) and Solvency (ex: Leverage ratios, ...), as well as some macroeconomic variables (ex: House Price Index, Mortgage Delinquency Rate, ...) available monthly. Details about the ratios used can be found [here](https://github.com/Gabriel-dLN/Project/blob/main/data_to_prepare/WRDS_Industry_Financial_Ratio_Manual.pdf).

Is added to these variables their first differences, which are noted with a 'd' in front of their name (e.g. dinv_turn instead of inv_turn). This is to take into account the effect of the variation of the variable on the risk of distress.


The date range is 01-01-2000 to 01-01-2010.

For the moment, all the covariates have been normalized company-wise.

Companies are labeled "in distress" if they were in a very bad economic condition at some point around 2007-2009. The distress category is composed by companies that went bankrupt, that got purchased of got a substancial bailout from the government.

It is subjective but fairly good for the study. For example, Lehman Brother filed for chapter 11 bankruptcy and liquidation whereas Bear Stearn was purhcased by JP Morgan but both went out of business, so putting them in different categories wouldn't make much sense. Same thing for companies that benefited from the Troubled Asset Relief Program (TARP): almost all the banks got a bailout (received financial assistance from their government) but for different reason like to pay their debt or in contrast to purchase a troubled company. 

# Model

The model used throughout this project is a **Time Dependant Cox Proportional Hazard Model**.
### Model specification

### Hypothesis

More on that [here](https://bmcmedresmethodol.biomedcentral.com/articles/10.1186/1471-2288-10-20).


# Approach

The data range used to fit the model is 06-30-2002 to 01-01-2010, to avoid the effects of the dot-com bubble. Each crisis is one of a kind with its own intrinsic phenomena, and this study aims at explaining the subprimes crisis.
### Potential biases
Thin limit between in distress and not.

# First results

The project is still in a research stage, but some interesting first results.

In the example below, a Time Dependant Cox Proportional Hazard Model is fitted on the data with the following covariates:
- debt_assets: Total Debt as a fraction of Total Assets.
- inv_turn: Inventory Turnover: COGS (Cost Of Goods Sold) as a fraction of the average Inventories based on the most recent two periods.
- dUNRATE: **Variation of** the unemployment rate. it represents the number of unemployed as a percentage of the labor force.<!--  Labor force data are restricted to people 16 years of age and older, who currently reside in 1 of the 50 states or the District of Columbia, who do not reside in institutions (e.g., penal and mental facilities, homes for the aged), and who are not on active duty in the Armed Forces.  -->

<!-- - rect_turn: Receivables Turnover: Sales as a fraction of the average of Accounts Receivables based on the most recent two periods -->
<!-- - ddebt_assets: **Variation of** Total Debt/Total Assets: Variation of the Total Debt as a fraction of Total Assets -->
<!-- - dinv_turn: **Variation of** Inventory Turnover: **Variation of** COGS as a fraction of the average Inventories based on the most recent two periods. -->


Using the table below, we can interpret the results:

Note that hazard ratios only make sense when compared across covariates. They cannot be interpreted alone.



![illustration](results/example_res.png "Illustration")

All the p-values are under 0.05. The Model is then statistically significant.

### Covariate Interpretations

#### 1. **deb_assets (Debt to Assets Ratio)**
   - **Coefficient** = 0.72
   - **Hazard Ratio (exp(coef))** = 2.06
   - **Interpretation**: An increase in the debt-to-assets (or leverage) ratio is associated with an increase in the risk of distress. Indeed, a higher leverage ratio leads to more risks. This aligns with typical financial intuition— companies with higher debt relative to their assets are more likely to face financial strain, especially during downturns.
   <!-- Add interpretation of what were the financial products that were assets and what were debts -->

#### 2. **inv_turn (Inventory Turnover)**
   - **Coefficient** = 0.50
   - **Hazard Ratio (exp(coef))** = 1.65
   - **Interpretation**: An increase in inventory turnover is associated with an increase in the risk of distress. It particularly makes sense as a lot of companies that went bankrupt were financing themselves using overnight REPOs. This implies a higher risk in case of a decrease of the liquidity or of a high market volatility environment.

#### 3. **dUNRATE (Variation of the Unemployement rate)**
   - **Coefficient** = 2.37
   - **Hazard Ratio (exp(coef))** = 10.70
   - **Interpretation**: An increase in the variation of the unemployement rate is associated with an increase in the risk of distress.

### Comparison of the hazard ratios (the exp(coef))
We notice that the variation of the unemployment rate is the primary factor of distress, followed by the leverage ratio, which is closely followed by the inventory turnover.

<!-- #### 2. **rect_turn (Receivables Turnover)**
   - **Coefficient** = -1.03
   - **Hazard Ratio (exp(coef))** = 0.36
   - **Interpretation**: A one point increase in receivables turnover (companies are collecting their receivables more quickly) is associated with a **64% decrease** in the risk of distress. This ratio illustrates the exposure to other financial institutions and the liquidity of the lending market. Companies stuggling with collecting debt will also struggle to issue some, as they are less solvable. -->

<!-- #### 1. **dinv_turn (Variation in Inventory Turnover)**
   - **Coefficient** = 0.38
   - **Hazard Ratio (exp(coef))** = 1.46
   - **Interpretation**: Higher variations in inventory turnover increases the risk of distress by **46%**. In fact, companies with a lot of short term debt are more vulnerable during liquidity crisis. -->

### Multicollinearity Consideration
The correlation matrix at the bottom shows relatively low correlations between the covariates (the maximum is 0.19). This suggests that multicollinearity is not a significant issue in the model, meaning the estimated effects of each covariate are likely reliable.

### Concordance Index
The concordance index is 0.23, which suggests the model has no predictive ability. While the coefficients are statistically significant and align with reasonable interpretations, the overall model’s ability to correctly rank companies by their risk of distress is low.

This isn't much of a problem as the model has no predictive purpose. Having a low concordance index but statistically significant variables is still relevant: the model shows that the 3 covariates have a real impact on the hazard ratio and so, the probability to be in distress.

Other effects can be responsible for the difficulty to obtain a model with high a concordance ratio:
- The data used for this study is reported quarterly so the duration between the beginning and end of the quarted isn't taken into account and companies categorized as in distress in the same quarter are noted the same day.
- The precision of the distress category is vague, so the order in which the companies go bankrupt is very approximate.
- A lot depends on human intervention, government policies and other events that are not quantifiable.
- Bad quality data.
- Sometimes the data of the companies that were in distress and those who were not are very similar, as each of them was highly impacted by the crisis. Adding companies from different sectors would not really make sense because they are different businesses so different ways of working and could dilute the relevant information.
