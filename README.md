# United States Obesity Analysis

The aim of this project was to understand what the drivers of obesity are in the US. The dataset analysed was from a [2019 US County Health Ranking census](https://www.countyhealthrankings.org/explore-health-rankings/rankings-data-documentation). An array of regression techniques were used to explore how obesity related to other physical and mental health indicators in the dataset. The final model highlighted that the % of people who are physically inactive, the population, the % of people who are food insecure and the % of people with a long commute are the best predictors of poor mental health. No policy recommendations are put forward in this analysis, however it would be worth policy makers investigating these factors further and in more detail, e.g. breaking down % of population who are physically inactive down further, e.g. into those who do <1hr exercise/week, those who do 1-5hours per week and those who do more than 5 hours etc. 



## Methods Used

- Data exploration
- Data visualisation
- Data cleaning
- Data transformation
- Regression (linear, polynomial, lasso, ridge, ElasticNet)
- Variance inflation factor analysis

## Technologies

- Python
- Pandas
- Numpy
- Matplotlib
- Seaborn
- Scikit-learn
- Yellowbrick
- statsmodels

## Executive Summary

The dataset contained physical and mental health information on the constituents of 3195 counties across the US. The aim was to establish the most relevant factors that can be used to predict obesity. The mean % of obese residents across all US counties was 32%, this is compared to a global obesity rate of 12%. The approach taken was to use regression models to establish how the multitude of physical and mental health features in the dataset correlate with obesity.

<p align="center">
  <img src="https://github.com/joekryan/obesity_analysis/blob/master/obseity_map.png" width=850>
</p>

## Data Exploration and Preprocessing

23 features were selected initially

           - '% Fair/Poor'
           - 'Physically Unhealthy Days'
           - 'Mentally Unhealthy Days'
           - '% Smokers'
           - '% Obese'
           - 'Food Environment Index'
           - '% Physically Inactive'
           - '% With Access'
           - '% Excessive Drinking'
           - 'PCP Rate' [PCP= primary care physician]
           - 'Dentist Rate'
           - 'MHP Rate' [MHP = Mental Health Professional]
           - 'Graduation Rate'
           - '% Some College'
           - 'Income Ratio'
           - 'Association Rate'
           - '% Severe Housing Problems'
           - '% Drive Alone'
           - '% Long Commute - Drives Alone'
           - Population
           - Unemployment Rate (2018)
           - Median Household Income (2018)
           - Average Unemployment Rate (2007-2018)
          
Many of the columns had null values, however these were not a high percentage of the total. Only 4 columns had more than 20 null values (out of 3142) and the highest number of null values was 245. These nulls were replaced with the state average. Additionally, there were two counties with small populations that had multiple null values aross several columns. These were dropped from the analysis.

## Models 

The data was split into a train dataset (2198 values) and a test dataset (942 values). A baseline regression model was then fit to the data that returned an R2 value of 0.59. Analysis of the cross validation scores showed that it was not overfitting to the data. 

Variance Inflation Factor with a threshold of 10 was then used for variable selection and to determine how much multicollinearity there was between the variables. This returned 9 variables. % Physically Inactive and %Food Inscure were not selected by VIF but were also included as 'common sense' factors. This led to a final selection of 11 variables:

  1.'% Physically Inactive'
  2. '% Food Insecure'
  3. 'PCP Rate'
  4. 'Dentist Rate'
  5. 'MHP Rate'
  6. 'Association Rate' 
  7. '% Long Commute - Drives Alone',
  8. '% Limited Access'
  9. '% Uninsured'
  10. 'Population'
  11. Unemployment_rate_2018'
 
The data was then transformed with a second degree polynomial transformation that would show variable interactions and increase complexity of the model. The data was then scaled using StandardScaler.

## Evaluation

Ridge, Lasso and Elastic Net were all used to optimise the hyperparamters of the regression model. Ridge Regression was optimised by iterating and crossvalidation. Lasso Regression was optimised with with Akaike's Information Criterion (AIC) and Bayesian Information Criterion (BIC), and Elastic Net was optimised using GridSearchCV.

Lasso (with BIC) had the highest R^2 value and will simplify our model (so better able to generalise) so this was chosen as the final model.

<p align="center">
  <img src="https://github.com/joekryan/obesity_analysis/blob/master/information_criterion.png" width=850>
</p>

## Final Model

<p align="center">
  <img src="https://github.com/joekryan/obesity_analysis/blob/master/final_model.png" width=850>
</p>

The final regression model had an R2 value of 0.6

The final model was then evaluated against the assumptions of linear regression. These are as follows:

1. Linear relationships between features and the target variable.
2. Residuals must be normally distributed.
3. There must be no multicolinearity in the data.

The first two assumptions were satisfied. There was a small degree of multicolinearity in the data, as two variables with a VIF score greater than 10 were included, however our model still broadly satisfies this third criterion. Multicolinearity would not affect the the accuracy of the model,regardless, however high degrees of multicolinearity would mean that the coefficients of the features are inflated/deflated and therefore not realistic. 

<p align="center">
  <img src="https://github.com/joekryan/obesity_analysis/blob/master/final_model_analysis.png" width=850>
</p>

Due to the polynomial transormation applied, our final model included interactions between coefficients and also the square of coefficients which can make it difficult to explain to clients - the produce of '%physically inactive' and '% drive alone' is not intuitive. Additionally, our regression returned both positive and negative coefficients, but often similar variables appeared as both positive and negative! For example, the coefficient with the higest postive value was '% Physically Inactive', but the coefficient with the largest negative value was '% Physically Inactive'^2. This is due to the polynomial interactions introducing a lot of colinear variables, which are being balanced by having negative and positive coefficients. 

<p align="center">
  <img src="https://github.com/joekryan/obesity_analysis/blob/master/positive_coef.png" width=850>
</p>
<p align="center">
  <img src="https://github.com/joekryan/obesity_analysis/blob/master/negative_coef.png" width=850>
</p>


## Recommendations
In this analysis no policies were put forward to reduce the obesity rate, however, this analysis does give the policy makers a place to start. They should investigate whether or not these features simply correlate with poor mental health or if the relationship is causal, and also look at these variables in more detail, take a more granular approach to, for example, the amount and type of physical activity that people do rather than jusst categorising people as inactive or not. This would provide better variables that would be better able to predict obesity.



