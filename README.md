# Analyzing Runtime and Money Spent on Snapchat Political Ads by Number of Impressions
Exploring [Snapchat's Political and Advocacy Ads Library](https://www.snap.com/en-US/political-ads/) data to find if there is any relationship between duration of runtime and number of impressions of political and advocacy advertising on Snapchat.
In order to get a comprehensive analysis of timing of political/advocacy ads throughout an entire year, I will be using the [2019 archives](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/PoliticalAds.csv).

## Industry Question:
Does how long a political/advocacy advertisement ran for in 2019 influence number of times it was delivered to users?

## Data Question:
Can the duration of runtime for a Snapchat political/advocacy advertisement predict the number of impressions? 

### Metrics 
* Snapchat defines **Impressions** as the number of times the Ad has been viewed by Snapchatters. 
* **Spend** is the amount in local currency that an advertiser has spent over the campaign up to current date. For consistency, all **Spend** data will be converted to USD. 
* **Duration** will be calculated using **Start Date** (time at which the Ad was set up to start delivering) and, where possible, **End Date** (time at which the Ad was set up to stop delivering). No information on **End Date** means that the Ads may be running indefinitely or until an Advertiser pauses the campaign [(1)](https://businesshelp.snapchat.com/en-US/article/political-ads-library). To get the most accurate results, we will be ignoring Ads with no specified **End Date**.
* For sake of precision and since Snapchat ads are usually short videos, **duration** of ad run-time will be measured in **seconds**.
See [documentation](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/2019PoliticalAds_readme.txt) or [FAQ](https://businesshelp.snapchat.com/en-US/article/political-ads-library) for more info.

## Data Interpretation
#### Duration of Runtime effect on Money Spent
![Duration of Runtime effect on Money Spent](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/data_visualizations/Duration%20on%20Spend.png)
* As shown in the chart above, the relationship between duration of runtime and the amount of money spent on airing the advertisement is very weak. The R^2 of 0.0141 implies that only about 1.41% of money spent could be predicted on Duration. 
#### Duration of Runtime effect on Impressions
![Duration of Runtime effect on Impressions](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/data_visualizations/Duration%20on%20Impressions.png)
* Similarly, there is a very weak relationship between duration of runtime and how many impressions it received. The R^2 of 0.0087 implies that only about 0.87% of Impressions could be predicted on Duration. 
#### Money Spent effect on Impressions
![Duration of Runtime effect on Money Spent](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/data_visualizations/Spend%20on%20Impressions.png)
* A much better indicator of Impressions, however, is how much money was spent to deliver the ads. As shown below, the R^2 of 0.7261 implies that roughly 72.61% of Impressions could be predicted on amount an advertiser paid to air the app. 
#### Correlation
![Correlation](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/data_visualizations/Correlation.png)
* The above is supported by the .85 correlation between Impressions and Spent (USD). In comparison, Duration and Spend (USD) have a 0.12 correlation and Duration and Impressions have a 0.09 correlation.
#### Regressions
![Duration and Spend Regression](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/data_visualizations/Regression.png)
* The multi-linear regression output gives us the following equation:
```Predicted Impressions = 199340.39 + (336.61*Spend(USD)) + (-0.014*Duration(sec))```
* This equation explains that as an advertiser spends more money on an ad, ... 
* The R Square value of 0.726 implies this model explains 72.6% of variations in Impressions. 
* The p-value of Duration is much larger than 0.05, however, which means that it is insignificant to predicting Impressions.
* Running the regression without Duration improves the regression model (as shown below). 
![Spend Regression](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/data_visualizations/Spend%20Regression.png)
* This regression output gives us the following equation:
```Predicted Impressions = 169027.22 + (336.25*Spend(USD))```
* The R Square value of 0.85 implies this model explains 85% of variations in Impressions. 

## Industry Answer & Findings
Overall, this data shows that duration of runtime of an ad is a poor indicator of how much money an advertiser paid Snapchat to air it and an even worse indicator of how many Snapchat users were delivered the ad. This was initially surprising because I had assumed that advertisements were priced depending on the amount of 'airtime' they received. Instead, Snapchat's advertisement structure prices ads per 1,000 impressions[(1)](https://strikesocial.com/blog/snapchat-cost/). If this was the only pricing model, however, number of Impressions and cost of app would have a much higher correlation and stronger linear relationship. Additionally, Snapchat also offers a "goal-based bidding" structure in which advertisers can set a maximum price for each time a user engages with the Ad beyond just viewing it. These "goal options" include when users swipe-up to access an associated link and app installs that advertisers can nake adjustable bets for. 


--- 

## Step-by-Step Instructions for Excel Data Analysis
* multiple linear  regression analysis with at least two indep variables for at least one agency 
* use manipulation tools like: split cells, arithmetic, If statements, VLOOKUP

1. Convert all **Spend** data to USD
* **Spend** data is currently in local currency, found in **Currency Code**.
* To convert **Spend** to USD, use the following formula:
```=IF(Currency Code="USD",Spend,IF(Currency Code="EUR",Spend*1.12,IF(Currency Code="GBP",Spend*1.29,IF(Currency Code="CAD",Spend*0.74,IF(Currency Code="AUD",Spend*0.66,"")))))```
* This formula applys the appropriate conversion rate depending on the associated **Currency Code** of either USD, EUR, GBP, CAD, and AUD. If none apply it leaves the cell blank.

1. Change format of **StartDate** and **EndDate** to be a valid date format.
* Current text format: yyyy/mm/dd hh:mm:ssZ 
* The 'Z' at the end refers to the timezone. It stands for Zulu time, aka Greenwich Mean Time (GMT).
* First, we must remove the Z from all Date cells using the _SplitCells_ function on the _Data_ tab. 
Select the **StartDate** data range and using the _Text to Columns_ functionality, select "Delimited" and click "Next >", set the delimiter of your data as "Other:" and write 'Z' and click "Next >", and leave the data format as "General" and select "Do not import column (Skip)" for column to the right. Click "Finished". Repeat this process for the **EndDate** data range.
* This should automatically update it to date format: m/d/yy h:mm 

2. Calculate Duration of Ad runtime
* To calculate total seconds between **EndDate** and **StartDate**, use the following formula:
``` =IF(NOT(ISBLANK(EndDate)),(EndDate-StartDate)*86400,"")```
* This formula calculates total seconds between **EndDate** and **StartDate** unless there is no specified **EndDate**, in which it leaves the **Duration** field blank. 
* 86400 is the number of seconds in one day (24 hrs * 60 min * 60 sec = 86400)
* A negative **Duration** time indicates an **EndDate** that happened before a **StartDate** which means the data is invalid because it is impossible for an Ad to have a negative runtime. To eliminate this data, use the _Sort & Filter_ functionality to sort **Duration** in "Ascending" order and delete any rows with negative **Duration**. Also delete any rows with an empty **Duration**.

3. Calculate correlation between **Spend**, **Duration**, and **Impressions**
* Using the _Data Analytics_ functionality in the _Data_ tab, choose "Correlation". Select **Spend (USD)**, **Duration**, and **Impressions** data ranges as "Input Range:". Make sure it's grouped by "Column" and check "Labels in First Row".

5. Linear Regressions of **Duration** with **Impressions** and **Spend**
* Select **Duration** then **Impressions** data ranges. Go the the _Insert_ tab and select _Scatter Plot_. 
* Ensure that **Duration** is on the x-axis and **Impressions** are on the y-axis using the _Select Data_ functionality on the _Chart Design_ tab. Also check that empty cells are shown as "Gaps" and that data in hidden rows and columns is not shown. 
* Now repeat this process with **Duration** and **Spend**
