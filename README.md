# Outage-Analysis
By Sivaram Ari and Mihir Joshi

## Introduction
The Dataset used in this project is Power Outages that occurred in the United States. We were curious if there was a pattern in the power outages that happened, and the question we had in the project is, Where and When do outages arise in America? Answering this question can provide insight into how these outages occur and could prevent them.

The Dataset has 1534 rows and 58 columns. The columns relevant to the analysis were Anomaly Level, Customers Affected, U.S. State, Ind.Customers, Year, Month, Climate Region, Cause Category, Outage start time, and Outage End Time.

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
The first step in data cleaning was to convert the Excel file into CSV and then remove empty rows and columns. Then, it was necessary to convert the Outage.Start and Outage.Restoration column into a datetime column and did so by calling the to_datetime function. Values are left as Nan because it felt better to maintain integrity of the dataset and not potentially create a trend that did not exist

<iframe
  src="assets/df.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

