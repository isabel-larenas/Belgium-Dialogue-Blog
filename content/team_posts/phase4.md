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

> The government model achieved an R² of 0.42 and MSE of 24.5.

---

## Model 1: Student Life Satisfaction Linear Regression

This model serves our student user persona with the goal of finding which European countries would offer the highest quality of life based on housing and environmental conditions. It predicts each country's **life satisfaction score** (1–10) for young adults aged 16–29 with tertiary (highest) education level based on housing indicators, and then **adjusts rankings based on the user's personal priorities**, so a student can see which countries best match what matters most to them.

### Data & Features
The model trains on `merged2.csv` (774 country-year-urbanisation rows across 30 European countries) created by merging five Eurostat tables on country, year, and degree of urbanisation. The target is `happy_rate`- mean life satisfaction from Eurostat's quality of life survey, filtered to ages 16–29 with tertiary education level. The features for this model are:

| Feature | Description |
|--|--|
| **Crime rate** | percent of the population reporting crime, violence, or vandalism in their area |
| **Noise rate** | percent of the population reporting noise from neighbours or the street |
| **Pollution rate** | percent of the population reporting pollution, grime, or other environmental problems |
| **HPI weight** | housing price index annual rate of change- how fast housing costs are rising or falling |
| **Degree of urbanisation** | dummy encoded as Cities (baseline), Towns & Suburbs, and Rural Areas within each environmental dataset |

The model also has four interaction terms of `crime × noise`, `pollution × noise`, `crime × HPI`, and `pollution × crime`, bringing the total to ten features. We included these interaction terms because a correlation matrix (see below) showed that the environmental features had moderate individual correlations with satisfaction, but their combined effects, such as high crime in a noisy area being worse than either alone, were not captured by the base features. Adding the interaction terms improved R² from 0.24 to 0.31.

{{< iframe src="plots/correlation_heatmap.html" width="100%" height="400" >}}

### Performance
The model explains **~31% of the variance** (the R²) in life satisfaction, based on our six features and their interaction terms. The **MSE of ~0.15** is relatively low given that satisfaction scores in the dataset range from 6.2 to 8.6. On average, the model's predictions are off by about 0.39 points on a 10-point scale. While an R² of 0.31 might seem modest, self-reported life satisfaction ratings can be influenced by many factors outside housing (such as employment, healthcare, cultural norms, political stability), so explaining over a quarter of the variation using just housing and environmental data can make a decent prediction model.

To check the model's assumptions, we looked at how the residuals compared against the predicted satisfaction scores. The residuals vs. fitted values plot showed a slight upward trend, suggesting that the linearity assumption is not perfectly met; the model tends to underpredict for countries with higher satisfaction and overpredict for lower ones. There was also a mild funnel shape indicating some heteroscedasticity. The residuals vs. order plot looked better, with no consistent trend across the index, suggesting autocorrelation is not a major concern. Overall, the model is reasonable as a ranking tool even if the linear assumptions are not perfectly satisfied.

<img src="/Belgium-Dialogue-Blog/images/linreg1.png" style="max-width: none !important; width: 500px !important;">
<img src="/Belgium-Dialogue-Blog/images/assump1_linreg.png" style="max-width: none !important; width: 500px !important;">

### How User Preferences Affect Rankings
Instead of simply providing the user with a static rating value, the student model adds a **preference layer** on top of the base predictions. The sliders on the prediction page represent how much each environmental factor matters to the user personally, ranging from 0 ("I don't care about this at all") to 100 ("this is extremely important to me").

When the user submits their preferences, the system first runs the trained model on each country's real Eurostat data to produce a base satisfaction score. It then applies a penalty to each country proportional to how bad that country performs on the factors the user cares about most:

    penalty_crime     = (slider_crime / 100) × (country_crime / max_crime)
    penalty_noise     = (slider_noise / 100) × (country_noise / max_noise)
    penalty_pollution = (slider_pollution / 100) × (country_pollution / max_pollution)
    penalty_hpi       = (slider_hpi / 100) × (country_hpi / max_hpi)
    adjusted_score = base_prediction − 2.5 × (sum of all four penalties)

This means two users with different priorities will see different country rankings from the same underlying model. A user who prioritizes low crime will see high-crime countries drop in the ranking, while a user who cares most about housing costs will see countries with rising prices penalised instead. The final scores are normalized to a 1–10 scale so the best country always shows as 10 and the worst as 1, making differences between countries immediately readable.

### What the Student Sees
In EuroHome, the Housing Satisfaction Predictor has two features:
- **Four preference sliders** where the student sets how much they care about crime, noise, pollution, and housing price growth, plus a toggle for their preferred area type (Cities, Towns & Suburbs, or Rural Areas).
- A **predicted satisfaction heatmap** where the student can visualize which countries best match their priorities, shaded from green (highest satisfaction) to red (lowest).
- An **ordered table** listing countries ranked by their adjusted predicted satisfaction score based on the student's stated preferences.



---

## Model 2: Government Agency Linear Regression

This model serves our government agency user persona with the goal of finding which European countries are in most need of housing funding. It predicts each country's **housing deprivation rate** which is the percent of people living in overcrowded housing or poor living conditions based on socioeconomic indicators, so a **higher prediction** creates a **stronger need for funding**.

### Data & Features
The model trains on `merged_linreg.csv` (642 country-year rows across 27 European countries, 2014–2023) created by merging Eurostat tables on country and year. The target is `deprivation_rate`. The features for this model are:

| Feature	| Description |
|--|--|
| **Immigration**	| total number of people that immigrated to the country in a given year |
| **Housing-cost overburden**	| percent of the population spending more than 40% of their disposable income on housing costs like rent, utilities, and maintenance |
| **GDP per capita** | the total economic output of a country divided by its population, used as a measure of a country's overall wealth and standard of living |
| **Population density** | the number of people living per square kilometer in a country |
| **Unemployment rate** |	percent of the working-age population that is actively looking for work but does not have a job |

The model also has three interaction terms of `density × unemployment`, `gdp × unemployment`, and `density × overburden`, bringing the total to eight features. I included these interaction terms because I used a correlation matrix (see below) to see how I could improve the model and did feature engineering, which improved the model by 5%.

{{< iframe src="plots/gov_correlation_heatmap.html" width="100%" height="400" >}}

### Performance
The model explains **~42% of the variance** (the r-squared) in housing deprivation, based on our five features and their interaction terms. The MSE of ~24.5 might seem high at first, but deprivation is measured on a 0 to 100 scale, so the errors are spread across a much larger range of values. On average, this comes out to the model being off by about 5 percentage points. This model is meant to rank which countries need the most support rather than predict an exact number, so I would say this level of accuracy is good enough for the purpose it holds.

To check the model's assumptions, we looked at how the residuals behaved against the predicted deprivation rates. The residuals stayed fairly balanced around 0 with no clear pattern as the fitted values changed, so linearity looks like a reasonable assumption here and the model is not leaning high or low at either end. The one thing we noticed is that the residuals were tightly clustered for countries with low predicted deprivation and became more spread out as the predicted values got higher. This points to some heteroscedasticity in the model, meaning its predictions are more consistent for lower-deprivation countries and less certain for higher-deprivation countries.

<img src="/Belgium-Dialogue-Blog/images/gov_linreg.png" style="max-width: none !important; width: 500px !important;">
<img src="/Belgium-Dialogue-Blog/images/gov_residuals.png" style="max-width: none !important; width: 500px !important;">

### What the Agency Sees
In EuroHome, the Housing Deprivation Predictor has two features:
- A **Predicted Deprivation Heatmap** where the government agency can visulize where housing funding is most needed based on which countries are indicated as a darker red.
- An **ordered table** listing in order the countries that need the most funding and its respective predicted housing deprivation rate.

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