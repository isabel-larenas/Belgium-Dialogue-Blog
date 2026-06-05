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

# ML Features

# ML exploration update

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
| student/train                        | NA                                      | adds the training data to database | | |
| student/test    | retrieves the mean squared error and r2                    | NA                                 | | |
| student/predict | NA                                                         | adds predicted data to database    | | |
| student/params  | retrieves student_id, scalar_std, scalar_mean, beta_values | NA                                 | | |

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