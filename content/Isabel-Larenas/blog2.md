---
title: "Blog Post 2"
date: 2026-05-27
draft: false
description: "Another test"
summary: Checkout my second blog post!
slug: "blog2"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "isabel-larenas"
showAuthorsBadges : false
---

# What I Worked On

For Phase 2 of the project, Laasya and I worked on creating a Proof of Concept ML Model. We both developed the API calls for each Eurostat dataset for crime, noise, pollution, and housing price index. Using these datasets, we ended up using a k-NN model to score European countries using a combination of safety and housing affordability.

After we pulled the datasets through the API, I did the initial exploratory data analysis (EDA) to get a sense of what we were working with by checking shapes, column names, null values, and the different geographic and categorical breakdowns in each dataset. From there, I cleaned the four datasets by standardizing column names, dropping nulls and EU-aggregate rows, and handling an extra purchase column in the HPI dataset. Once everything was in a consistent format, I aggregated each dataset down to country–year level and merged them into a single feature table that we could feed into the model.

I also created the data visualizations to see what the cleaned data looked like. I overlaid histograms comparing the distributions of noise, pollution, and crime, a line chart showing how EU averages have changed over time, per-country bar charts of the most recent year, box plots broken down by degree of urbanization, and a multi-line plot of housing price trends per country. These helped us understand the data before modeling and helped us interpret results for our project.

# Experiences Abroad

Since Phase 1, I enjoyed going to the Atomium which was very cool to look at from afar but I was a bit disappointed that one of the spheres was under maitenance. I also had a lot of fun today going to Ghent and learning the history behind the church through a VR experience!