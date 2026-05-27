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