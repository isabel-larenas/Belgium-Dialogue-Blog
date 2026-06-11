---
title: "Project - Phase IV"
date: 2026-06-11
draft: false
description: "Project updates for phase IV"
summary: "Our completed project"
slug: "phase4post"
tags: ["project"]
authors:
  - "geo-thatch"
  - "isabel-larenas"
  - "maira-padani"
  - "laasya-gattu"
showAuthorsBadges: false
showTableOfContents: true
---

#
# Project Updates and Feedback

After getting feedback from Phase III, we fine-tuned some routing and UI functions, reorganized our routing blueprints, expanded our REST API matrix, and gathered design feedback for implementing into the final phase of our project, making it presentation and deployment ready. Phase IV was the final phase of our project, where we brought together all of these components and more to execute our application into a fully functioning web app. We specifically focused on building out our front end pages, refining our ML models, finalizing the database schema, and ensuring that our app was user-friendly, cohesive, and varied in the way that we present data and features. Below we describe the key changes and additions made during this phase. 

---

# Software Architecture

## Final Database Schema

Our final database consists of 11 tables: 
- `country`
- `user`
- `university`
- `listing`
- `favorites`
- `reviews`
- `funding`
- `funding_draft`
- `social_indicator_types`
- `social_indicator_stats`
- `student_model_params`
- `gov_model_params`

The newest tables are `gov_model_params` to support the deployment of our newest ML model, **Housing Deprivation Predictor** on the Government Agency Dashboard, and `favorites` to support the student user feature of favoriting a listing. 

The `student_model_params` and `gov_model_params` tables were added to store trained ML model parameters (beta values, scaler mean, and scaler standard deviation) so that predictions can be displayed without retraining on every request. This was a key architectural decision that allowed us to separate training from inference. 

We added `created_at` and `updated_at` audit fields to tables where users can create features, such as reviews and favorites in the student pages, listings in the real estate pages, and funding drafts in the government agency page. This internal audit allows us to track when drafts were created and last modified.

Our stimulated data was also a significant addition in this phase. Tables such as `listings`, `favorites`, `reviews`, and the `funding_draft` tables were populated with much more mock data. Unlike the `funding` table which holds real historical funding data, `funding_draft` is user-generated and tied to a specific `user_id`, which allows government agency users to propose and manage their own funding plans independently on their own account. This also applies to the other tables just mentioned.

## API Architecture

Our REST API is built with Flask and organized using the Blueprint pattern. In Phase IV, we refactored the API to give each entity its own blueprint instead of routing everything through a single `housing_bp`. The final file structure is:

- `housing.py` — country, user, university routes 
- `listing.py` — listing CRUD and favorites
- `reviews.py` — review CRUD
- `funding.py` — funding and funding draft CRUD
- `stats.py` — social indicator types, stats, and Eurostat sync routes
- `prediction.py` — student and government ML model routes

Each blueprint is registered with `url_prefix="/housing"` in `rest_entry.py`. This makes the codebase significantly more maintainable and easier to debug, since each file has a single responsibility and routes are much more organized.

## REST API Matrix Updated 

Counting all routes across all blueprint files:

- housing.py — 5 routes
- listing.py — 7 routes
- reviews.py — 4 routes
- funding.py — 8 routes
- stats.py — 14 routes
- prediction.py — 9 routes

**Total**: approximately 47 routes

### Matrix
| Route | GET | POST | PUT | DELETE |
|-------|-----|------|-----|--------|
| `/country` | Retrieve country/ies | NA | NA | NA |
| `/user` | Retrieve user(s) | NA | Update user information | NA |
| `/university` | Retrieve university/ies | NA | NA | NA |
| `/listing/cities` | Retrieve cities from all listings | NA | NA | NA |
| `/listing` | Retrieve listing(s) | Create new listing | Update listing | Delete listing |
| `/listing/<id>` | NA | NA | Update listing | Delete listing |
| `/favorites` | Retrieve saved listings | Save a listing | NA | Remove saved listing |
| `/reviews` | Retrieve review(s) | Create new review | NA | NA |
| `/review/<id>` | NA | NA | Update review | Delete review |
| `/funding` | Retrieve funding | Create funding record | Update funding record | Delete funding record |
| `/funding/<id>` | NA | NA | Update funding record | Delete funding record |
| `/funding-draft` | Retrieve funding drafts | Create funding draft | NA | NA |
| `/funding-draft/<id>` | NA | NA | Update funding draft | Delete funding draft |
| `/social-indicator-types` | Retrieve indicator types | NA | NA | NA |
| `/social-indicator-stats` | Retrieve all stats | NA | NA | NA |
| `/social-indicator-stats/pollution` | Retrieve pollution data | Sync from Eurostat | NA | NA |
| `/social-indicator-stats/crime` | Retrieve crime data | Sync from Eurostat | NA | NA |
| `/social-indicator-stats/poverty` | Retrieve poverty data | Sync from Eurostat | NA | NA |
| `/social-indicator-stats/overcrowding` | Retrieve overcrowding data | Sync from Eurostat | NA | NA |
| `/social-indicator-stats/noise` | Retrieve noise data | Sync from Eurostat | NA | NA |
| `/social-indicator-stats/hpi` | Retrieve house price index data | Sync from Eurostat | NA | NA |
| `/student/train` | NA | Train student model | NA | NA |
| `/student/test` | Retrieve MSE and R² | NA | NA | NA |
| `/student/prediction` | NA | Return predicted satisfaction score | NA | NA |
| `/student/params` | Retrieve stored model parameters | NA | NA | NA |
| `/government/train` | NA | Train government model | NA | NA |
| `/government/test` | Retrieve MSE and R² | NA | NA | NA |
| `/government/prediction` | NA | Return predicted deprivation score | NA | NA |
| `/government/deprivation-map` | Retrieve all country deprivation predictions | NA | NA | NA |
| `/government/params` | Retrieve stored model parameters | NA | NA | NA |


## Docker and Deployment

The application runs as three Docker containers: `mysql_db`, `web-api`, and `web-app`. We built a sync system where Eurostat data is automatically fetched from the API on first page load using Streamlit's `session_state`, and the ML models are trained via a POST request before use. 

## How the Containers Connect
The three containers communicate over a shared Docker network which is defined in docker-compose.yaml. The web-app container runs Streamlit and makes HTTP requests to the web-api container using the internal hostname web-api:4000, which is why all API calls in the Streamlit pages use http://web-api:4000/housing/ instead of localhost. The web-api container runs Flask and connects to mysql_db using the credentials defined in the .env file. The mysql_db container initializes the schema and seed data by running the SQL files in database-files/ on startup.

This architecture means the frontend, backend, and database are fully independent. Changes to the Streamlit pages don't require a rebuild, but changes to Flask routes or the database schema require docker compose down -v and docker compose up --build to be deployed to the frontend. 

---

# ML Models

| | Student LinReg | Government LinReg |
|--|--|--|
| **R²** | 0.31 | 0.42 |
| **MSE** | 0.14 | 24.5 |
| **Target** | Life satisfaction | Housing deprivation |
| **Features** | Crime, noise, pollution, HPI, urbanization | Immigration, overburden rate, GDP, density, unemployment |


---

> The student model achieved an R² of 0.31 and MSE of 0.14.

> The government model achieved an R² of 0.42 and MSE of 6.91.

## Model 1: Student Linear Regression





---

## Model 2: Government Agency Linear Regression




---

# Application Walkthrough

## Student Persona

The student persona includes three main pages. 

**View Listings** - The listings page allows students to filter by country, city, property type, price range, and an associated university. Each listing shows its title, location, property type, rent, and average rating. Students can click into any listing to view anonymous reviews and ratings left by previous students.

**Saved Listings** — Allows students to save listings they are interested in and revisit them later.

**Housing Satisfaction Predictor** — Users can input their preferences of social indicators with sliders and get a ranked table of their predicted life satisfaction scores out of 10 in each country, giving them a sense of quality of life in different housing conditions.

## Real Estate Agent Persona

The real estate agent persona includes three main pages:

**Market Dashboard** — Shows housing market trends across the EU, including pricing and listing data by country and property type.

**View Listings** — Agents can browse and manage their own property listings with filters for country, city, and property type.

**Create Listing** — Agents can post new properties by filling out a form with the listing title, country, city, price, property type, and associated university if applicable. 

## Government Agency Persona

The government agency persona includes five main pages:

**Explore Funding Programs** — A searchable table of historical EU housing funding programs which can be filtered by country and agency. 

**My Funding Drafts** — A personal dashboard where government agency users can view, edit, and delete their own proposed funding plans. Drafts are user-specific, so each agent only sees their own submissions. The page includes success banners for both edits and deletions.

**Create Funding Draft** — A specific page for proposing new funding plans, with fields for program name, country, amount, targeted social indicators, targeted demographics, and a description. 

**Risk Heatmap** — An interactive Folium map shading EU countries by social indicator risk (pollution, crime, poverty, overcrowding, noise, or house price index). The map includes a side-by-side bar chart ranking countries by the selected indicator. Both the map and rankings update based on the selected indicator and year. Data is pulled live from Eurostat via our sync routes. Users can also download the map as an HTML file and the rankings as a CSV. 

**Housing Deprivation Predictor** — An ML-powered page showing predicted housing deprivation rates by country, which is powered by the government linear regression model. This gives agency workers a data-driven view of which countries need the most housing support. 

---


# Reflections

This project gave us hands-on experience building a full-stack application from scratch, integrating real external data sources, training and deploying ML models, and managing a collaborative repository with Git. Some of the most challenging aspects were the Eurostat data sync pipeline (each dataset had a different structure and dimension schema), managing Docker rebuilds efficiently during development, and handling merge conflicts across multiple branches and personas. The main obstacle was designing a cohesive UI that can execute our user stories from backend to front and presenting it in a creative, interactive, and useful way.

If we were to continue this project, there are several areas we could expand or improve on. We would focus on adding user authentication, making the app more secure, and would allow features like funding drafts and saved listings to be truly personal to a user's account. On the ML side, we would explore non-linear models for the student satisfaction predictor — the current linear regression has an R² of 0.31, which suggests the relationship between housing indicators and life satisfaction is not fully linear. We would also expand the government agency persona further by adding the ability to approve or publish funding drafts, so that proposed plans could shift from draft status to adding to the database of official funding records. Finally, we would improve the UI across all personas with more consistent styling, mobile responsiveness, and richer data visualizations. For example, time series charts would be useful to visualize how social indicators have changed over the years for each country. 