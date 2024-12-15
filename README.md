# Unwrapping Customer Delight: Using Machine Learning to Optimize Surprise Gift Strategies
### Fall 2024 AI Studio Project for BTTAI | Team Estée Lauder 1A
---
## Motivation
Surprise gift campaigns, or surprise and delight marketing, are a strategy utilized by many brands that aims to exceed customer expectations by providing unexpected positive experiences. Ideally, these gifts increase customer loyalty and future spending with the business. Since the gifts are given at a cost to the seller, however, it is important to consider their actual return on investment for the business, as poor implementation of a surprise gifting strategy can be detrimental to the company. So, does receiving a surprise gift influence future customer spending behavior?

## Overview
For our project, we worked alongside senior data analysts at Estée Lauder to obtain a simulated dataset of customer spending information for 2021 and 2022, based on real consumer data. For this dataset, consumers received a surprise gift if they spent over $80. We used this data to attempt to understand the causal effect of surprise gifts (given in 2021) on future customer spend (in 2022), with the following goals:
- Provide quantifiable estimates of a surprise gift's effect (eg., ROI) on future purchases
- Obtain credible intervals to account for uncertainty within results

## Data Preparation & Exploratory Data Analysis
The dataset which we were provided was relatively simple, consisting of only 20000 rows and 3 columns: 
- `customerId` (string, primary key)
- `'Dollars Spent 2021'` (float)
- `'Dollars Spent 2022'` (float)

Considering that we are only working with a single data file, `customerId` was quickly dropped as it contained no relevant information. For our remaining two columns, we used `'Dollars Spent 2021'` as a feature (prior spend), and `'Dollars Spent 2022'` as the label (future spend). For the purposes of our models, we decided to create two additional features: `Gift`, a boolean variable indicating whether `'Dollars Spent 2021' > 80` (and by extension, whether the customer received a surprise gift); and `Interaction`, an interaction term for `Gift` and `'Dollars Spent 2021'`, which is useful considering the obvious dependence between the two variables.


## Methodology
To import our data, download the parquet file from the `data\` folder in this repository, or run the following code:
```
import pandas as pd

datafile = "https://github.com/estee-lauder-1a/ai-studio-project/blob/main/data/CustomerData.parquet?raw=true"
df = pd.read_parquet(datafile, engine="auto")
```
We used both frequentist and Bayesian (subjective) probabilistic approaches to obtain a more comprehensive understanding of our data. Since our simulated data did not involve A/B testing and is based on historical data, we determined the best method for determining a surprise gift's effect would be to use a pseudo-experimental model: Regression Discontinuity Design. To build our models, we used the `statsmodels` library for our frequentist OLS model, and the `NumPyro` library for our Bayesian MCMC model.

## Results
Model Coefficients and 95% Confidence/Credible Intervals
Term/Coefficient | OLS Model Value | Bayesian Model Value (Mean) 
--- | --- | ---
Intercept | 11.91 [8.83,15.00] | 11.94 [8.79,15.03]
Prior Spend (2021) | 1.07 [1.02,1.12] | 1.07 [1.02,1.12]
Treatment (Gift) | 23.01 [15.86,30.16] | 22.94 [15.83,30.23]
Interaction (Prior Spend * Treatment) | -0.21 [-0.30,-0.13] | -0.21 [-0.30,-0.13]
Standard Deviation | 9.97 | 9.98 [9.79,10.17]

## Acknowledgements
This project was the combined work of our AI Studio Team, including Taabeer, Pranavi, and Vivian, but we could not have done it without the help of so many people. Thank you to everyone at Estée Lauder for providing us with the tools and data we needed. To our Challenge Advisors, Eddy and Luis, and our TA Yi, thanks for your support and guidance. Finally, thank you to Break Through Tech, the Cornell Tech AI Program team, and especially Erika and Abby for an amazing program experience so far. We can’t wait to see what the Spring AI Studio brings next! 
