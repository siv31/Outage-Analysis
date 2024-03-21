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
Null Hypothesis: The proportions of Anomalies in winter months are the same
Alternate Hypothesis: There are fewer anomalies during winter months compared to other season months
Test Statistic: Proportion of anomalies occuring in winter compared to the generated sample distribution of 0.25 for every season
Calculate P-value = Calculated by seeing if the proportion of sample is less than or equal to proportion of anomolies in winter
P-value = 0.506 fail to reject the null hypothesis

## Framing a Prediction Problem
We are predicting the column Cause. Category and using a multiclass classification. The reason we chose Cause.Category because wanted to potentially understand how to fix or stop power grid issues by accurately predicting the cause and the metric we are choosing to evaluate by accuracy because we want to see the ratio of the number of correctly predicted over the total number of predictions
