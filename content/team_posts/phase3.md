---
title: "Project - Phase III"
date: 2026-06-03
draft: false
description: "Project updates for phase III"
summary: "Began building our web-app and continued work on our ML models"
slug: "phase3post"
tags: ["project"]
authors:
  - "geo-thatch"
  - "isabel-larenas"
  - "maira-padani"
  - "laasya-gattu"
showAuthorsBadges: false
---
# Project Updates
Since phase II of our project, we have changed some aspects of our DDL, like removing attributes (max_distance_km and max_budget), adding attributes (country_codes, created_at, and updated_at). Removing max_budget is because of the structure of our wireframes– we have sorting filters for students on the listings page itself, and so it doesn't necessarily make sense to close off all listings above a certain budget. This change may not be permanent, and it may be necessary once we start building out the users and what data they may want to save. Max_distance_km was removed because on the EU scale, we are unable to pinpoint exact neighborhoods or addresses, so any specific distance is hard to track. “Attributes for basic audit traits”, or the created_at and updated_at attributes of the funding, listing, and review entities, were added as a result of our phase II feedback. We found that country_codes were necessary when dealing with our Eurostat data, since they have no specific index or country_name attribute in the original API. We also added an entity funding-drafts, which contains all user-created funding plans which can be posted to the site. This is because the funding data is set as ‘real’ funding that has been contributed to certain regions, while a funding-draft is simply an opportunity to plan where funds may be useful. 

# Updated ML Models
Since Phase 2, our ML models changed fairly significantly. Using the feedback provided, we decided to update our K-Nearest Neighbors model, which was previously a recommender system for the student persona. We decided to modify it to be a linear regression model. Instead of providing a list of recommended countries to move to, the linear regression model takes in user inputs of ideal crime rate, noise rate, pollution rate, expected housing price growth (long term), and degree of urbanization, and outputs a prediction of what the expected satisfaction level may be to give users a sense of quality of life in different areas. 

To implement this updated model, we sourced a new dataset from Eurostat, [overall life satisfaction by age, sex, and education](https://ec.europa.eu/eurostat/databrowser/view/ilc_pw01$dv_2761/default/table?lang=en&category=qol.qol_lif.qol_life_sat), which gave us the values on which we could train the model on and then run a prediction. We then conducted the same data cleaning and merging processes as we did on the previous datasets. 

When training the model, we had a few difficulties acheiving a good r2 score, and ended up settling with an r2 of 0.27. While not ideal, we experimented with different feature engineering strategies to see if that number could improve. Although we attempted squaring column values to try polynomial features and interaction terms (such as crime x degree of urbanization_rural, noise x degree of urbanization_towns and suburbs, etc.), but not much seemed to have an impact. We stuck with the initial features of crime_rate, noise_rate, pollution_rate, hpi_weight, and deg_urb (dummy versions of variable) because these seemed the most relevant to user preferences and made the most sense in terms of the model performance. 

We also ran assumption checks on this model. The residuals vs. y hat values plot shows a clear upward trend; as the fitted values increase, the residuals also tend to increase, which means the model may be  underpredicting for higher satisfaction scores and overpredicting for lower ones. This indicates the linearity assumption is violated and suggests the relationship between the housing features and life satisfaction may not fully captured by a linear model. There also appears to be slight heteroscedasticity because the variance of the residuals is not constant across the range of y hat values. However, the residuals vs. order plot looks much better. The residuals appear randomly scattered around 0 with no consistent upward or downward trend across the index, suggesting that autocorrelation is not occuring within this model.
![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/assump1_linreg.jpeg)
![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/assump2_linreg.jpeg)
Overall, we may need to do some further fine tuning on this model to improve the metrics, but the progress has been good so far.

## 2nd ML Model


# REST API matrix
|                                      | GET                                     | POST                     | PUT                     | DELETE                   |
| ------------------------------------ | --------------------------------------- | ------------------------ | ----------------------- | ------------------------ |
| /country                             | retrieve country/ies                    | NA                       | NA                      | NA                       |
| /user                                | retrieve user(s)                        | NA                       | update user information | NA                       |
| /university                          | retrieve university/ies                 | NA                       | NA                      | NA                       |
| /social_indicator_types              | retrieve types of social indicator data | NA                       | NA                      | NA                       |
| /social_indicator_stats/pollution    | retrieve pollution data                 | Sync data from eurostat  | NA                      | NA                       |
| /social-indicator-stats/crime        | retrieve crime data                     | Sync data from eurostat  | NA                      | NA                       |
| /social-indicator-stats/poverty      | retrieve poverty data                   | Sync data from eurostat  | NA                      | NA                       |
| /social-indicator-stats/overcrowding | retrieve overcrowding data              | Sync data from eurostat  | NA                      | NA                       |
| /social-indicator-stats/noise        | retrieve noise data                     | Sync data from eurostat  | NA                      | NA                       |
| /social-indicator-stats/hpi          | retrieve house price index data         | Sync data from eurostat  | NA                      | NA                       |
| /listing                             | retrieve listing(s)                     | create new listing       | update old listing      | delete old listing       |
| /listing/cities                      | retrieve city/ies from all listings     | NA                       | NA                      | NA                       |
| /review                              | retrieve review(s)                      | create new review        | update old review       | delete old review        |
| /funding                             | retrieve funding                        | NA                       | NA                      | NA                       |
| /funding-draft                       | retrieve funding drafts                 | create new funding draft | update funding-draft    | delete old funding-draft |


This table represents all routes used in our project. For a parameter like country, the users can only interact with the data through reading, since the list is set and cannot be changed in any way. However, for listings, reviews, or funding-drafts, the databases can be added to, updated, or deleted at any time. The user table can be updated, but with our limited knowledge we aren’t working with user creation or deletion currently. Country, user, university, and listing/cities routes are all used primarily for filtering in both real estate and student personas. Social_indicator_stats are largely used in the government agency persona, for both the tables and the heat map. Reviews are viewable for the student, after clicking on a specific listing. Funding is an accessable data set of previous money allocated towards housing or community programs, and funding-drafts is a chance for a government agent to plan out a potential future funding plan for a specific country. 

# Mocked app and functionality

![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/web-app1.png)
This is our listings page which is available under the student role. On top, there are categories by which the user can sort all listings– country, property type, price range, associated universities, and cities. Each listing has a title, city, country, property type, rent, and an average rating, when data allows. Listings which have universities have it displayed. From here, users can go directly to the reviews to see comments and ratings which previous renters have left. 

![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/web-app2.png)
All reviews on posts are anonymous in order to protect users' privacy. When leaving a review, comments are required and the rating is optional, so that users can leave comments which shouldn’t affect the rating either way, like if furniture is included or the neighborhood pool is a block away. 

![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/web-app3.png)
This page is under the government_agency persona and shows the previous funding that has been allocated to housing and communities across the EU. With this, agencies can compare and make decisions about what funding has been the most effective and in which areas. It will be an important consideration in decision making about sending funds, especially when money can be tight and a little bit of money may go a long way. 

![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/web-app4.png)
Further down on the same page is where government agencies can look at social indicators across the EU over time. This is where they can compare funding and how it has affected countries. Additionally, they may draft a funding plan which can be uploaded to the website for everyone to see. With this, many people will be able to communicate their ideas to the public and potentially help the government overall come to a conclusion. 

![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/web-app5.png)
This screen is a visual representation of a social-indicator (in this case, pollution) across the EU. This is a useful map which can show which areas the EU should direct more focus to when it comes to climate initiatives in the future. 

![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/web-app6.png)
This table shows the same data as the map, but in a more specific way, with the percentages listed next to the countries, which are ranked in order of highest to lowest. Again, this is a good way to show which countries are struggling the most in any given area, and where future funding should be directed. 

# Conclusion
This week, we made significant progress towards are end goal of a fully functioning web-app. Now that we have routes, building the rest of the app is primarily reliant on the streamlit code, which means that the bulk of the data processing with the API is finished. In terms of the ML models, we have one fully successful ML model and one proof of concept to continue working on in this next week. Next week, we will be fully expanding to complete all of our wireframes and hopefully fulfill the last of our user stories. 