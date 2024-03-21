# Outage-Analysis
By Sivaram Ari and Mihir Joshi

## Introduction
The dataset used in this project is Power Outages that occurred in the United States. We were curious if there was a pattern in the power outages that happened, and the question we had in the project is, Where and When do outages arise in America? Answering this question can provide insight into how these outages occur and could prevent them.

The dataset has 1534 rows and 58 columns. The columns relevant to the analysis were Anomaly Level, Customers Affected, U.S. State, Ind.Customers, Year, Month, Climate Region, Cause Category, Outage start time, and Outage End Time.

- ANOMALY.LEVEL: The ONI index tracks El Niño and La Niña events based on sea surface temperature anomalies
- CUSTOMERS.AFFECTED: The count of customers that were affected by power outages
- U.S. State: The state where the power outage happened
- IND.CUSTOMERS: Yearly number of customers that were served in industrial electricity
- YEAR: Year power outage took place
- MONTH: Month power outage took place
- CLIMATE.REGION: U.S Climate Regions 
- CAUSE.CATEGORY: Categories of all events causing power outages
- OUTAGE.START.TIME: The time the outage started
- OUTAGE.END.TIME: The time the outage finished

## Data Cleaning and Exploratory Data Analysis
The first step in data cleaning was to convert the Excel file into CSV and then remove empty rows and columns. Then, it was necessary to convert the Outage.Start and Outage.Restoration column into a datetime column and did so by calling the to_datetime function. Values are left as NaN because it felt better to maintain integrity of the dataset and not potentially create a trend that did not exist

<iframe
  src="assets/df.html"
  width="800"
  height="600"
  frameborder="0"
> </iframe>

This is a histogram plot of Anomaly levels to evaluate if there was an anomaly level that appeared the most, which was -0.3, appearing 191 times, and the histogram also appears to be skewed to the right.
<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
> </iframe> 

This is a bivariate plot of the Anomaly level over Outage.Start Year. There appears to be a pattern of average anomaly level being higher in the latter years than the previous years.
 <iframe
  src="assets/bivariate.html"
  width="800"
  height="600"
  frameborder="0"
> </iframe> 

| CLIMATE.REGION     |   CUSTOMERS.AFFECTED |
|:-------------------|---------------------:|
| Central            |             127552   |
| East North Central |             138389   |
| Northeast          |             121960   |
| Northwest          |              81420   |
| South              |             183854   |
| Southeast          |             182644   |
| Southwest          |              39028.9 |
| West               |             194580   |
| West North Central |              47316   |

This table aggregated by mean customers affected in each climate region shows the region of customers that were most affected by power outages. 

## Assessment of Missingness
We believe that the NMAR Column in the Outage dataset is Customers Affected because it depends on the population and some additional data that could be used to make this into MAR data by having more information about the historical trends to have more data on the missingness. 

The test statistic conducted was the absolute difference in sample means between columns of IND.Customers and Customers Affected to see if the Customers Affected missingness depends on the IND.Customers column, and after conducting the test, a p-value of 0.046 was achieved suggesting that 
 <iframe
  src="assets/missing.html"
  width="800"
  height="600"
  frameborder="0"
> </iframe> 
This is the empirical distribution of the test statistic used which again is absolute difference in sample means  and the observed test statistic is 4512.66
 <iframe
  src="assets/empirical.html"
  width="800"
  height="600"
  frameborder="0"
> </iframe> 

## Hypothesis Testing
- Null Hypothesis: The proportions of Anomalies in winter months are the same
- Alternate Hypothesis: There are fewer anomalies during winter months compared to other season months
- Test Statistic: Proportion of anomalies occuring in winter compared to the generated sample distribution of 0.25 for every season
- Calculate P-value = Calculated by seeing if the proportion of sample is less than or equal to proportion of anomolies in winter
- P-value = 0.506 fail to reject the null hypothesis
  
Test 2
- Null Hypothesis:The average number of anomalies in the first 10 years is the same as the average number of anomalies in the last 7
- Alternate Hypothesis: The average number of anomalies in the last 7 years is higher than the average number of anomalies in the first 10 years
- Test Statistic: Difference in the average number of anomalies that occurred each year in the first 10 and last 7
- Calculate P-value = Calculated by seeing if the proportion of sample is less than if it were to be sampled evenly
- P-value = 0.01 reject the null hypothesis in favor of the alternate

## Framing a Prediction Problem
We are predicting the column Cause. Category and using a multiclass classification. The reason we chose Cause.Category because we wanted to potentially understand how to fix or stop power grid issues by accurately predicting the cause and the metric we are choosing to evaluate by accuracy because we want to see the ratio of the number of correctly predicted over the total number of predictions

## Baseline Model
The baseline mode is the random forest, and the features we used are anomaly level, climate region, and month.
 - Climate.Region: We OneHotEncoded this column because it was qualitative and we dropped the first column to not have multicollinearity
 - Month and anomaly-level: These columns were left alone as these were quantitative.

This model's accuracy score was 0.48, which we thought was okay but could definitely improve. If we added more relevant features, we feel like the score would improve.

## Final Model
To improve our baseline model to be more accurate at predicting the cause of the power outages, we decided to add three more features to our final model.

 - "OUTAGE.START.TIME" : We decided to add this feature to our next model because we saw a co-relation between the time that the outage started and the Anomaly Level. Because of this co-relation, we believed that outage start time could be related to the reason that the outage started in the first place.
We used a custom transformer to Binarize the time based on AM and PM to categorize Afternoon and Evening vs Morning.
- "CUSTOMERS.AFFECTED" : The customers affected by each outage could also be a predictor of the cause of the outage itself. The factors related to an outage could be higher in urban and suburban areas compared to rural ones due to the locations of cities across the country.
This was a numerical column so we left it as it was
- "CLIMATE.CATEGORY" : The climate plays a large role in the efficient transportation of electricity. Based on environmental conditions, states often impose regulations on how much electricity can be used or transported at a time. The current environment can give us clues into what caused the outage. We One Hot Encoded this column because it was a categorical column. We dropped the first to prevent Multicollinearity

To choose the best model, we were debating between a Decision Tree and a Random Forest Classification model. We decided to choose a Random Forest because it combined the best aspects of a Decision Tree while averaging it out to ensure the best predictions

We then used GridSearchCV to test which value would be best for the max depth hyperparameter and how many estimators would be best for the n_estimators hyperparameter. The test resulted in us finding that a depth of 7 and 110 estimators gave us the best validation score.

While the baseline model had a score of just 0.48, the final model has a score of 0.87. This was a significant improvement in the score of the final model over the baseline model. The score for the baseline model was likely so low because we were under fitting the training data.

## Fairness Analysis
In regards to our final model, we decided to test whether there was a significant difference in the precision rate depending on the season and temperature outside. 

Our Null Hypothesis: The precision would be the same across warmer and colder months (Months 12, 1, 2, and 3). 
Our Alternate Hypothesis: The precision would be lower for colder months than warmer months.

We created a column to group by whether it was a colder or warmer month. We then calculated the precision score on each subset and took the difference between the scores as the test statistic. We decided on a significance level of 0.05 for this test.

After running our permutation test, we got a p-value of 0.049, which is less than our signidicance threshold. Our conclusion is that we reject the null in favor of the alternate. This means that there is a bias in our model that makes it less precise at predicting the cause of outages during the colder months.
