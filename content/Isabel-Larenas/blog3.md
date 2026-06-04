---
title: "Blog Post 3"
date: 2026-06-04
draft: false
description: "Another test"
slug: "blog3"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "isabel-larenas"
showAuthorsBadges : false
---

# What I Worked On

For Phase 3 of the project, Laasya and I worked on improving the proof-of-concept model from phase 2 by adding a dataset to the model about life satisfaction. In phase 2, our model was using KNN, but we decided to switch to using linear regression model to give a more personalized prediction. Also from Phase 2, I updated the screenshots of the plots that we had to actual interactive plots. 

Furthermore, I started designing and building the second model that we need for the project. This ML model targets Emma's persona, a government agency project manager who needs to identify which EU countries require the most housing support. Using the Eurostat datasets of immigration inflow and severe housing deprivation rate, I built a linear regression model that predicts a country's housing deprivation rate based on immigration patterns.

To prepare the data, I called the Eurostat API for both datasets, cleaned them by dropping EU-level aggregates and keeping only total population rows, and merged them by country and year. For feature engineering, I log-transformed the immigration counts since the raw values were really skewed by large countries like Germany, created a one-year lag so the model uses last year's immigration to predict this year's deprivation, and added a dummy variable for each country to account for the fact that each country has its own baseline deprivation level. The model was trained using the normal equation and got an R² of 0.9816 and an MSE of 6.9064. x

# Experiences Abroad

Since Phase 2, I enjoyed going to the chocolate factory and trying quality chocolate! Personally, I love chocolate as it is one of my favorite flavors.

<img src="/Belgium-Dialogue-Blog/images/chocolate.jpg" alt="chocolate" style="max-width: none !important; width: 500px !important;">