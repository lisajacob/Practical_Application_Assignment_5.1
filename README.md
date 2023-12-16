# WILL A DRIVER ACCEPT THIS COUPON?

[Lisa's Repository](https://github.com/lisajacob/Practical_Application_Assignment_5.1/)

My company, The Rad Couponistas, wants to build a coupon recommendation system. They've tasked me to create feature profiles that will most likely accept coupons based on data collected via an Amazon Mechanical Turk survey published on the UCI website.

## Exploratory Data Analysis
The features are categorical. Despite this, some features have a natural order from low to high, such as the number of bar visits per month, income, time of day, age, or temperature, etc.. 74 duplicate rows were removed. Removed the excess whitespace from the income column. Missing values percentages are as follows:
```sh
car                     99.14%
Bar                     0.85%                                  
CoffeeHouse             1.72%                                  
CarryAway               1.19%
RestaurantLessThan20    1.02%                                  
Restaurant20To50        1.5%                                 
```
The car column did not have enough data for analysis. The data is categorical, and the mode (most frequent value) did not alter the data distribution. I've used the mode value as a substitution for the missing values in the other columns and verified that the overall distribution was unaffected. 
- Dropped car
- Substituted NaN in Bar with "never"
- Substituted NaN in CoffeeHouse with "less1"
- Substituted NaN in CarryAway with "1~3"
- Substituted NaN in RestaurantLessThan20 with "1~3"
- Substituted NaN in Restaurant20To50 with "less1"

##### Features
Looking closely at the data distribution, I narrowed down which category had the highest acceptance ratios in each feature. It was also important to determine which categories appeared most often. Ideally, we'd want to pick a feature or combination of features which have both a high frequency and high acceptance ratio. 
| Column | Most Frequent values | Highest Acceptance ratio | Lowest Acceptance Ratio|
| ---    |---| ---                      | ---                    | 
|education       |Some college - no degree, Bachelors degree| Some High School (72%) |Graduate degree (Masters or Doctorate) (52%) |
|coupon          |Coffee House| Carry Out & Take Away (73%) Restaurant<20 (71%) | Bar (41%)|
|time            |7AM, 6PM| 2PM (66%) 10AM (61%) | 7AM (50%)|
|occupation      |Unemployed| Healthcare Support (70%), Construction & Extraction (69%), Healthcare Practitioners & Technical (68%) | Retired (46%)|
|destination     |No Urgent Place| No Urgent Place (63%) |Work (50%) |
|passenger       |Alone| Friend(s) (67%) | Kid(s) (50%)|
|weather         |Sunny| Sunny (59%) | Rainy (46%)|
|temperature     |80| 80 (60%) | 30 (53%)|
|age             |21| below21 (63%) 21(60%) 26(60%) |50plus (50%) |
|expiration      |1d| 1d (62%) | 2h (50%)|
|gender          |Female, Male| Male (59%) | Female (55%) |
|maritalStatus   |Married partner, Single| Single (60%) | Widowed (48%) |
|hasChildren     |0| 0 (59%) |1 (54%) |
|income          |$25000 - $37499| [$50000 - $62499, $25000 - $37499], Less than $12500] (59%) | $75000 - $87499 (48%) |
|Bar             |never| 4~8 (64%), 1~3(62%) | never (53%) |
|CoffeeHouse     |less1| 1~3 (65%), 4~8 (63%) | never (46%) |
|CarryAway       |1~3| [1~3, 4~8] (58%), gt8 (57%) | less1 (50%)|
|direction_same  |0| 1 (58%] | 0 (56%)|                                           
|direction_opp   |1| 0 (58%] | 1 (56%) |
|toCoupon_GEQ15min    |1| 0 (61%) | 1 (53%)|                                                |toCoupon_GEQ25min    |0| 0 (59%)  | 1 (41%)|
|RestaurantLessThan20 |1~3| gt8 (61%) | less1 (53%)|                               
|Restaurant20To50     |less1| [4~8, gt8] (66%) | never (51%)|                              

###### correlation analysis
- toCoupon_GEQ5min is not correlated to anything, even Y. This is because there is only one value 1. We can ignore or drop this column for the rest of the analysis.
- direction_same and direction_opp are perfectly correlated on every point. We do not need to analyze both columns and can drop one.

#### Findings
##### Summary
To summarize, summer is the best time to run a marketing campaign when the temperature is 80 degrees. The most lucrative coupons are those for Carry out & Take away or for cheap restaurants for meals under $20. The best time of day to send these coupons is 10 AM, 2 PM, and 6 PM to people who carry away between 1 to 8 times a month. High outcomes are also associated with drivers who have no Urgent Work and have no Kid(s) riding with them. The restaurants are preferred to be between 15 and 25 minutes away. Ideally, the coupon should have at least a 1-day expiration time. The only exception is for Coffee Houses, where a 2-hour expiration time is acceptable. Bar coupons do not do well. If you do market Bar coupons, its best to send to those drivers who visit 1 to 8 times a month. Avoid sending Bar coupons those aged 50 and over. To promote Coffee House coupons, its best to do so to customers age 21 and 26 who visit 1 to 3 times a month. We need to gather more data for education group Some High School. They have high potential for accepting coupons for carry out and cheap restaurants. We should also gather data for Marital Status of Widowed or Divorced as there is insufficient data for accurate analysis.
Gender is not a good feature for determining high acceptance as the acceptance ratio is close for both possible outcomes. Similarly, Direction Same or Direction Opp are not good features.

Next steps: We should categorize the occupation into smaller buckets as there are too many categories for sparse data. Discard gender, direction same as they have similar acceptance rejection ratio for their categories.

##### Details
- Acceptance Ratio for all observations: 57%
- Coupons with the sharpest deltas between acceptance and rejection are "Carry Out & Take Away" and "Restaurant<20", with acceptance rates at 73% and 71% respectively. Coupons for Bar have the lowest acceptance rate at 41%
- Coupons are most like to be accepted in 80 degree temperature when the weather is sunny
- Drivers who visit a bar are more likely to accept coupons if they visit between 1 and 8 times a month. 
- The majority of drivers who never visit bars are using coupons for "Carry out & Take away" and "Restaurant(<20)"
- Acceptance Ratio for folks with more than 3 bar visits a month: 62% vs 56% for fewer than 3 visits per month
- Acceptance Ratio for drivers who go to a bar more than once a month 62%. Age does not contribute significantly when drivers go more than once a month. However, those drivers who go less than once a month and are over 25 don't accept a lot of coupons.
- The acceptance rate is higher for driver who go to cheap restaurants 4 or more times a month
- The acceptance rate is higher when the there are no Kids as passengers
- The acceptance rate is higher for drivers who are below 30.
- Acceptance rate for drivers who go to bars more than once a month and had passengers that were not a kid and had occupations other than farming, fishing, or forestry: 62% All others: 54%
- Married people do not frequent bars as much as Single people. Their distribution falls mostly in the never or less1 category of bar visits per month. The data is skewed towards married and single people in comparison to folks who are divorced and widowed. So the dataset cannot be used as a reliable predictor of customer behavior for the Divorced and Widowed categories.
- "Carry out & Take away" or “Restaurant(<20)” coupons when received at 10AM or 2PM or 6PM is favorable with a 79% acceptance rate vs 50% for all others.
- Drivers prefer coupons with 1 day expiration; “Restaurant(<20)” has the lowest rejection for 1 day expirations. Coupons for "Coffee House" are okay with 2-hour expiration coupons. Coupons for “Restaurant(<20)” have a high rejection rate for 2-hour expiration. Factoring the expiration period in, improves the acceptance ratio to 86% vs 52% for all others
- The highest acceptance percent is associated with an education category "Some High School". Based on the histplots , it is apparent that the survey does not have sufficient sample data for that category. So, it could be coincidence that this education category has a high acceptance ratio. Or it would be good to conduct a survey focused specifically on that group, to confirm that the acceptance ratio is indeed valid.
- For "Carry out & Take away" or "Resturant(<20)" coupons, drivers who carried away between 1~8 times with a coupon for < 25minutes away had an acceptance ratio of 76% whereas, Others: 50%
-People age 21 and 26, who visit the coffee houses less than 1 time or 1 to 3 times a month have a higher coupon acceptance rate for coffee






