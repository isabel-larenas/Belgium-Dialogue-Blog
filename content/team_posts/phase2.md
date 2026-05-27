---
title: "Project - Phase II"
date: 2026-05-26
draft: false
description: "Project Updates"
summary: "Extended design report including data collection, EDA, ML models, and wireframes."
slug: "phase2post"
tags: ["project", "Setup"]
authors:
  - "geo-thatch"
  - "isabel-larenas"
  - "maira-padani"
  - "laasya-gattu"
showAuthorsBadges: false
---
# Project Updates
Since Phase 1, we have updated each of our personas (student, real estate investor, government agency) and adapted our project plan by making our user stories and platform functions more comprehensive. Below are the specific updates for each user persona, along with documented changes to user stories **(bolded)**.

# User Personas
## Student- Noah Bernard
**Noah is a 21-year old international student from Belgium. He is looking to relocate to a different European country to complete his Master’s degree attending university in a large city. Both quality of life and proximity to institutions are priorities for his housing search. His first time leaving home to live independently while working part-time, he is navigating the housing market with a tight student budget. His parents come from a lower-middle class background and can only provide minimal support for rent.**

- As a student, I want to be able to cap prices at my maximum budget and see which neighborhoods offer the best safety level within that price range, so that I don't waste time looking at places I can't afford. **I also want to save these filtered results to a favorites list so I can revisit them later without re-entering my criteria. I also want to review listings I may have visted or seen.**

- As a student, I need to commute to a specific campus daily, so I want to see a residence’s distance to my university's address and filter results to a range of different neighborhoods within a walkable or short transit distance, so that I only see options that are actually practical for my daily routine. **I also want to save preferred listings to a favorites list so I can easily compare them later.**


## Real Estate Agent- James Keen
James Keen is a real estate agent with a European agency called “Strike Realty”. He has been with Strike since graduating from KU Leuven 12 years ago at 22. Now, James is 34, and has risen higher up in the company. He has been tasked with narrowing down the best markets to target across Europe. He lives in Brussels, where Strike is based, with his wife and 3 daughters. 

- As a real estate agent, **I want to view platform data on user search trends and listing engagement** to identify where the most people are interested in migrating, so I can allocate resources to high-demand areas. 

- As a real estate agent, I want to consider the areas where housing is needed to support the growing population so I can maximize profits. **I also want to create new listings in high-demand areas to expand my agency's portfolio where people can review, like, or comment on them.** 

## Government Agency Project Manager- Emma Maria Berg 
Emma Maria Berg is a 30 year old project manager at the European Social Fund Plus (ESF+). 
She graduated from the College of Europe in Bruges with a masters’ in Social Policy. Her work focuses on allocating the ESF+ fund to certain communities and distributing those to local programs. In order to create an outline of a designated budget to provide inadequate areas with the proper resources and accommodations, Emma needs a tool that can provide information on poverty, housing costs and overcrowding so that the member states’ managing authorities and policy analysts can make decisions on the best areas to allocate necessary funds.

- As a project manager, I want to see overcrowding by demographic type so we can allocate funds to the most vulnerable groups. **I also want to draft potential funding plans, publish them to the platform, and allow users to like or upvote plans to gauge public support.** 

- As a project manager, I want to track how affordable housing has changed over the years so I can track whether funding allocation is actually helping populations and regions in the EU that need it. **I also want to save country comparisons so I can revisit and reference them over time.**



# Localized ER Models 
## Student 
<img src="/Belgium-Dialogue-Blog/images/student-er.png" alt="Student ER Diagram" style="max-width: none !important; width: 1000px !important;">

The student ER model is based on a student entity with attributes such as user_id, name, email, max_budget, and max_distance_km. Each student is enrolled in one university (M:1), with partial participation on the student side and total participation on the university side, meaning a university must exist but a student may not yet be enrolled. The university entity is located in a country (M:1), with partial participation for university and total for country. Listings also exist within a country (M:1) and can have zero or many reviews (1:M), with partial participation for listings and total for reviews, since every review must belong to a listing. The country entity connects to social_indicator_stats (1:M) with total participation on both sides, and each social indicator stat record maps to one social_indicator_type (M:1), with total participation for stats and partial for types.

## Real Estate Agent 
<img src="/Belgium-Dialogue-Blog/images/real-estate-er.png" alt="Real Estate ER Diagram" style="max-width: none !important; width: 1000px !important;">

The agent ER model is based on an agent entity with attributes such as user_id, name, and email. An agent creates listings (1:M), with total participation on the agent side, meaning every agent must have at least one listing. Each listing is located in a country (M:1) and can have zero or many reviews (1:M), with partial participation for both listing-to-reviews and listing-to-country. The country side follows the same social indicator structure: country has many social_indicator_stats (1:M) with total participation, and each stat is of one social_indicator_type (M:1). 

## Government Agency Project Manager
<img src="/Belgium-Dialogue-Blog/images/gov-er.png" alt="Government Agency (Project Manager) ER Diagram" style="max-width: none !important; width: 1000px !important;">

The government agency ER model is based on a government_agency entity with attributes such as user_id, manager's name, and email. Government agencies have a M:N relationship with country, with partial participation on both sides, meaning an agency can be associated with zero or many countries and vice versa. A separate funding entity with attributes funding_id, year, amount, program, and agency belongs to a country (M:1), with partial participation for funding and total for country. The country entity again connects to social_indicator_stats (1:M) with total participation, and each stat maps to a social_indicator_type (M:1). 

## Global ER Model 
<img src="/Belgium-Dialogue-Blog/images/global-er-model.png" alt="Global ER Diagram" style="max-width: none !important; width: 1000px !important;">

- The global model unifies all three personas into a single user entity aggregated by a role attribute. Depending on their role, a user can enroll in a university (M:1), create listings (1:M), belong to a country (M:1), or be associated with funding records (1:M). Participation is partial on most of these relationships, reflecting that not every role uses every connection. Country serves as the central hub, linking to listings and universities through "is in" relationships, to social_indicator_stats (1:M, total on both sides), and to funding (1:M). Each stat maps to a social_indicator_type (M:1). Listings can also have reviews (1:M), with total participation for reviews and partial for listings.
- This design also helps to keep the schema flexible enough to support students searching for housing, agents managing listings, and government agencies tracking social indicators and funding.

# Global DDL First Pass
<img src="/Belgium-Dialogue-Blog/images/DDL-model.png" alt="DDL Model (First Pass)" style="max-width: none !important; width: 1000px !important;">

## DDL SQL 
    DROP DATABASE IF EXISTS practice;
    CREATE DATABASE IF NOT EXISTS practice;
    USE practice;


    CREATE TABLE country (
        country_id INTEGER PRIMARY KEY,
        country_name VARCHAR(30) NOT NULL
    );

    CREATE TABLE social_indicator_types (
        sit_id INTEGER PRIMARY KEY,
        name VARCHAR(100) NOT NULL
    );

    CREATE TABLE social_indicator_stats (
        stats_id INTEGER PRIMARY KEY,
        country_id INTEGER NOT NULL,
        sit_id INTEGER NOT NULL,
        year YEAR,
        value DECIMAL,
        unit VARCHAR(50),

    CONSTRAINT fk_stats_country FOREIGN KEY (country_id) REFERENCES country (country_id),
    CONSTRAINT fk_stats_sit FOREIGN KEY (sit_id) REFERENCES social_indicator_types (sit_id)
    );

    INSERT INTO social_indicator_types (sit_id, name) VALUES
    (1, 'Pollution'),
    (2, 'Crime'),
    (3, 'Poverty'),
    (4, 'Overcrowding'),
    (5, 'Noise'),
    (6, 'House Price Index'),
    (7, 'Under-occupied');


    CREATE TABLE university (
        university_id INTEGER PRIMARY KEY,
        country_id INTEGER NOT NULL,
        university_name VARCHAR(75) NOT NULL,
        city_name VARCHAR(30),
        address VARCHAR(250),

    CONSTRAINT fk_uni_country FOREIGN KEY (country_id) REFERENCES country (country_id)
    );

    CREATE TABLE user (
        user_id INTEGER PRIMARY KEY,
        university_id INTEGER,
        country_id INTEGER,
        name VARCHAR(100),
        role VARCHAR(50),
        email VARCHAR(100) UNIQUE,
        max_budget DECIMAL,
        max_distance_km DECIMAL,

    CONSTRAINT fk_user_country FOREIGN KEY (country_id) REFERENCES country (country_id),
    CONSTRAINT fk_user_uni FOREIGN KEY (university_id) REFERENCES university (university_id)
    );


    CREATE TABLE listing (
        listing_id INTEGER PRIMARY KEY,
        country_id INTEGER,
        associated_university_id INTEGER,
        user_id INTEGER,
        price DECIMAL,
        property_type VARCHAR(50),
        city_name VARCHAR(50),

    CONSTRAINT fk_listing_country FOREIGN KEY (country_id) REFERENCES country (country_id),
    CONSTRAINT fk_listing_uni FOREIGN KEY (associated_university_id) REFERENCES university (university_id),
    CONSTRAINT fk_listing_user FOREIGN KEY (user_id) REFERENCES user (user_id)
    );

    CREATE TABLE reviews (
        review_id INTEGER PRIMARY KEY,
        listing_id INTEGER NOT NULL,
        rating INTEGER,
        comment VARCHAR(2000),

    CONSTRAINT fk_reviews_listing FOREIGN KEY (listing_id) REFERENCES listing (listing_id)
    );

    CREATE TABLE funding (
        funding_id INTEGER PRIMARY KEY,
        country_id INTEGER NOT NULL,
        year YEAR,
        amount DECIMAL,
        program VARCHAR(100),
        agency VARCHAR(100),

    CONSTRAINT fk_funding_country FOREIGN KEY (country_id) REFERENCES country (country_id)
    );

    CREATE TABLE ml_model (
        country_id INTEGER PRIMARY KEY,
        year YEAR,
        noise_rate_scaled DECIMAL,
        pollution_rate_scaled DECIMAL,
        crime_rate_scaled DECIMAL,
        hpi_weight_scaled DECIMAL,
        safety_score DECIMAL,
        safety_index DECIMAL,
        affordability_index DECIMAL,
        combined_score DECIMAL,

    CONSTRAINT fk_ml_country FOREIGN KEY (country_id) REFERENCES country (country_id)
    );


# Wireframes
## Home Screen
<img src="/Belgium-Dialogue-Blog/images/homewf.jpg" alt="Home Screen/Landing Page" style="max-width: none !important; width: 500px !important;">

## Student Screen 
### 01
<img src="/Belgium-Dialogue-Blog/images/studentwf01.jpg" alt="Student Screen Page 01" style="max-width: none !important; width: 500px !important;">

### 02
<img src="/Belgium-Dialogue-Blog/images/studentwf02.jpg" alt="Student Screen Page 02" style="max-width: none !important; width: 500px !important;">

## Real Estate Screen 
### 01 
<img src="/Belgium-Dialogue-Blog/images/realestatewf01.jpg" alt="Real Estate Page 01" style="max-width: none !important; width: 500px !important;">

### 02
<img src="/Belgium-Dialogue-Blog/images/realestatewf02.jpg" alt="Real Estate Page 02" style="max-width: none !important; width: 500px !important;">

## Government Agency Screen 
### 01 
<img src="/Belgium-Dialogue-Blog/images/govwf01.jpg" alt="Government Agency Page 01" style="max-width: none !important; width: 500px !important;">

### 02
<img src="/Belgium-Dialogue-Blog/images/govwf02.jpg" alt="Government Agency Page 02" style="max-width: none !important; width: 500px !important;">



# DS ML Model Proof-of-Concept

Since Phase 1, we updated a few of our datasets to ones that provided more relevant data to what we were looking for, but they still were the same concept. We decided to replace the initial crime, violence, or vandalism dataset with one that was [separated by degree of urbanization](https://ec.europa.eu/eurostat/databrowser/view/ilc_mddw06__custom_21597102/default/table) to stay consistent with the other three datasets. We also replaced the housing price index dataset with one that was [sorted by annual data](https://ec.europa.eu/eurostat/databrowser/view/prc_hpi_a/default/table?lang=en&category=prc.prc_hpi.prc_hpi_inx), so the time data would be consistent with the other three datasets that also had time in years.

For the first ML model we decided to create a k-Nearest Neighbors model that would work as a recommender system, to help student users decide their country of interest through inputting their ideal levels of noise, pollution, crime, and budget to recieve the k number of European countries that best suit their requirements. We decided to use the 4 datasets on noise, pollution, crime, and housing price index to train this model.

### API Calls and Data Cleaning

We conducted the API calls through the Eurostat base link and using the individual dataset codes (publicly available) to access them. For the EDA and data cleaning, we went through the raw data and calculated some summary statistics to get a sense of the data before we manipulated it. For the cleaning, we mainly focused on dropping null rows and calculating total crime, noise, and pollution rates.
<img src="/Belgium-Dialogue-Blog/images/eda.png" style="max-width: none !important; width: 700px !important;">
<img src="/Belgium-Dialogue-Blog/images/eda2.png" style="max-width: none !important; width: 700px !important;">

For initial data visualizations, we hoped to see patterns among crime, pollution, and noise rates and their respective countries. This information would help inform the main questions driving our project, which is finding ideally where in Europe housing is actually affordable and livable based on environamental factors and personal preferences, and how that has changed over the years.

- Line chart of EU average data shows how crime, pollution, and noise change over time realtive to each other. Crime and noise are slowly dropping, but pollution has more variation. This provides a high-level overview of what the rates looked like in the past, and may be beneficial to creating a prediction model for prediction future noise, crime, and pollution rates overall.
<img src="/Belgium-Dialogue-Blog/images/edaviz1.png" style="max-width: none !important; width: 700px !important;">

- Overlayed histogram visulizations of the noise, pollution, and crime rates so we could compare their distributions side by side. All three rates are right-skewed, meaning most countries cluster at lower rates with a few high outliers. Crime has the tightest distribution, making it possibly the most accurately predictable feature.
<img src="/Belgium-Dialogue-Blog/images/edaviz9.png" style="max-width: none !important; width: 700px !important;">

- Horizontal bar charts per country for the most recent year for moise, pollution, and crime. Luxembourg and Austria consistently rank high for both noise and crime. Albania ranks lowest across all three metrics but that may reflect underreporting rather than truly better conditions, as it falls at the bottom in all three plots. Romania is also a clear outlier for pollution at 44%.
<img src="/Belgium-Dialogue-Blog/images/edaviz2.png" style="max-width: none !important; width: 700px !important;">
<img src="/Belgium-Dialogue-Blog/images/edaviz3.png" style="max-width: none !important; width: 700px !important;">
<img src="/Belgium-Dialogue-Blog/images/edaviz4.png" style="max-width: none !important; width: 700px !important;">

- Box plots split by degree of urbanization to check whether cities, towns, and rural areas report different rates. Cities are significantly worse than towns and rural areas across all three metrics, with much wider variance. This suggests that an urbanization filter by degurba (degree of urbanization) = Cities/Towns and Suburbs/Rural areas for student users would be beneficial to separate country/national averages from more local reality.
<img src="/Belgium-Dialogue-Blog/images/edaviz5.png" style="max-width: none !important; width: 700px !important;">
<img src="/Belgium-Dialogue-Blog/images/edaviz6.png" style="max-width: none !important; width: 700px !important;">
<img src="/Belgium-Dialogue-Blog/images/edaviz7.png" style="max-width: none !important; width: 700px !important;">

- A multi-line Housing Price Index plot with overlapping lines for each country. Almost every country's prices shot up well past the inital 2010 level. Turkey's spike is an outlier and might need to be be removed. Once filtered, the chart will be useful for the real estate agent's investment model to identify countries with steady housing price index growth.
<img src="/Belgium-Dialogue-Blog/images/edaviz8.png" style="max-width: none !important; width: 700px !important;">


### Preliminary ML Model: K-Nearest Neighbors
From the individual cleaned datasets, we aggregated the data from each into an individual noise_rate, crime_rate, and pollution_rate that would set us up for the model. For example, in the noise table we used groupby to group together all columns aside from country and year, then average all of the noise_rate across them, so we got one totaled noise_rate value per country per year. This was one of our challenges initially because merging the tables without aggregating the values would have given us a row explosion in our final csv files.

After this, we merged the datasets into one table and did some feature engineering to create the noise, crime, and pollution rate, as well as the safety and affordability scores that make up the combined score that users will get recommendations based on. The safety score was calculated with the mean of noise + pollution + crime, and affordability was calculated by the housing price index. Additionally, we scaled the data using standard scaler to make sure our data was formatted consistently. We also had to flip the index of the scores so a lower initial score would return as a positive number, so users would interpret it as a good match.
<img src="/Belgium-Dialogue-Blog/images/mergedcsv.png" style="max-width: none !important; width: 700px !important;">

We trained and tested the data using a 80/20 split and refit the scaler on the training data. Then we calculated the cosine similarity and l2-norm distances to find the best k for our model, and these are the plots the model gave us.

<img src="/Belgium-Dialogue-Blog/images/cosineknn.jpeg" style="max-width: none !important; width: 600px !important;"><img src="/Belgium-Dialogue-Blog/images/l2knn.jpeg" style="max-width: none !important; width: 600px !important;">

Based on these graphs, we determined that the best k was 5, and found the metrics that showed us the success of the model. We used the mean squared error, which was reported as 0.1212, which means our model only has about 12% error, and the r2 value was 0.6131, which means that our model captured 61% of the variance of the data. While not ideal, these metrics show a decent success rate for a first attempt. We hope to improve this if possible.

### Future Phase III
For our second model, we're thinking of using a linear regression model with Eurostat's housing cost overburden rate dataset, combined with demographic info, to follow housing affordability over time. We want to see which demographic factors are affecting affordability, to help government agency managers create thier funding plans to target areas needing development. For Phase III, we will need to pull and clean the two unused datasets, run similar cleaning processes, and train the linear regression model.
