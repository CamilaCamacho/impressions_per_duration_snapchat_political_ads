# Analyzing Success of Snapchat Political Ads by Number of Impressions for Duration
Exploring [Snapchat's Political and Advocacy Ads Library](https://www.snap.com/en-US/political-ads/) data to find if there is any relationship between timing and number of impressions of political and advocacy advertising on Snapchat.  

## Industry Question:
Can number of impressions or cost for a political/advocacy advertisement in 2019 be predicted based on how long it ran for?

## Data Question & Metrics:
In order to get a comprehensive analysis of timing of political/advocacy ads throughout an entire year, I will be using the [2019 archives](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/PoliticalAds.csv).

### Metrics 
See [documentation](https://github.com/CamilaCamacho/timing_of_impressions_snapchat_political_ads/blob/master/2019PoliticalAds_readme.txt) for more info.
* Snapchat defines **Impressions** as the number of times the Ad has been viewed by Snapchatters. 
* **Spend** is the amount in local currency that an advertiser has spent over the campaign up to current date. For consistency, all **Spend** data will be converted to USD. 
* **Duration** will be calculated using **Start Date** (time at which the Ad was set up to start delivering) and, where possible, **End Date** (time at which the Ad was set up to stop delivering). No information on **End Date** means that the Ads may be running indefinitely or until an Advertiser pauses the campaign.[(1)](https://businesshelp.snapchat.com/en-US/article/political-ads-library). To get the most accurate results, we will be ignoring Ads with no specified **End Date**.
* For sake of precision and since Snapchat ads are usually short videos, **duration** of ad run-time will be measured in **seconds**.

### Step-by-Step Instructions for Excel Data Analysis
* multiple linear  regression analysis with at least two indep variables for at least one agency 
* use manipulation tools like: split cells, arithmetic, If statements, VLOOKUP
0. For easier data analysis, copy and paste the following data columns into new sheet or workbook:
* ADID
* CreativeURL
* Currency Code
* Spend
* Impressions
* StartDate
* EndDate
* OrganizationName

1. Change format of **StartDate** and **EndDate** to be a valid date format.
* Current format: yyyy/mm/dd hh:mm:ssZ 
* The 'Z' at the end refers to the timezone. It stands for Zulu time, aka Greenwich Mean Time (GMT).
* First, we must remove the Z from all Date cells using the _SplitCells_ function on the _Data_ tab. 
To do this create an empty column to the right of **StartDate**. Select the **StartDate** data range and using the _Text to Columns_ functionality, select "Delimited" and click "Next >", set the delimiter of your data as "Other:" and write 'Z' and click "Next >", and leave the data format as "General" and click "Finished". Repeat this process for the **EndDate** data range.
Please note that this process eliminates the 'Z' without adding anything to the empty columns we created to the right of it but we still needed to create those empty columns so as to not overwrite any existing data. You may delete these empty columns.
* Now, we must change the Date cells into a recognizable Date format. Select the **StartDate** and **EndDate** data columns, using the _Number Format_ editor on the _Home_ page that says "General" and click "More Number Formats...". Select "Custom" and as "Type:" write 'yyyy/mm/dd hh:mm:ss' and click "OK". 

2. Calculate Duration of Ad runtime
* To calculate total seconds between **EndDate** and **StartDate**, use the following formula:
``` =IF(ISBLANK(EndDate),"",(EndDate-StartDate)*86400)```
* This formula calculates total seconds between **EndDate** and **StartDate** unless there is no specified **EndDate**, in which it leaves the **Duration** field blank. 
* 86400 is the number of seconds in one day (24 hrs * 60 min * 60 sec = 86400)
* A negative **Duration** time indicates an **EndDate** that happened before a **StartDate** which means the data is invalid because it is impossible for an Ad to have a negative runtime. To eliminate this data, use the _Sort & Filter_ functionality and unselect any negative numbers and blanks.

3. Convert all **Spend** data to USD
* **Spend** data is currently in local currency, found in **Currency Code**.
* To convert **Spend** to USD, use the following formula:
```=IF(Currency Code="USD",Spend,IF(Currency Code="EUR",Spend*1.12,IF(Currency Code="GBP",Spend*1.29,IF(Currency Code="CAD",Spend*0.74,IF(Currency Code="AUD",Spend*0.66,"other")))))```
* This formula applys the appropriate conversion rate depending on the associated **Currency Code** of either USD, EUR, GBP, CAD, and AUD. 

4. Linear Regression of **Duration** (aka length of Ad runtime in seconds) and number of **Impressions**
* Select **Duration** then **Impressions** data ranges. Go the the _Insert_ tab and select _Scatter Plot_. 
* Ensure that **Duration** is on the x-axis and **Impressions** are on the y-axis using the _Select Data_ functionality on the _Chart Design_ tab. Also check that empty cells are shown as "Gaps" and that data in hidden rows and columns is not shown. (See Data Visualization Below)

5. Correlation Between **Spend**, **Duration**, and **Impressions**
* Using the _Data Analytics_ functionality in the _Data_ tab, choose "Correlation". Select **Spend**, **Duration**, and **Impressions** data ranges as "Input Range:". Make sure it's grouped by "Column" and check "Labels in First Row".


## Data Answer
* perform calculations and model-building to answer data-related questions 
### Data Visualization (at least 1)
* at lest one excel chart that ilustrates simple multiple linear regression with only one of the selected indep variables that has labeled best fit line, R^2 value, x&y axes, title
### Links to outside sources

## Industry Answer & Findings
### Summarizing findings (>250 words)
### Data Interpretation
* relate data findings back to initial business question and outline what your linear regression model tells us about election ads, ... 
* What do your findings mean and why might this be important? Highlight your confidence in your predictions based on your linear regression models, how this info should be used in practice, and what additional data might be used for further analysis. 
