---
title: "Phase 3 Blog Post"
date: 2026-06-03
draft: false
description: "Phase 3 Updates"
slug: "blog3-maira"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "maira-padani"
showAuthorsBadges : false
---
# What I worked on
We made a lot of progress on building out our UI and wireframes, as well as connecting stimulated data and sourced data from the database. On the backend, I built out the POST and GET routes for social indicator statistics, pulling real data from Eurostat's API across six different datasets: pollution, crime, poverty, overcrowding, noise, and house price index. Each dataset had its own structure and dimension schema, which made the sync logic pretty difficult. For example, the noise dataset had six dimensions including degree of urbanisation and poverty threshold, requiring a custom filtering to extract a single value per country per year. The house price index used a different dataset entirely (prc_hpi_a) with a base year index that needed the right unit filter to return real values.
I also built the funding draft routes (POST and GET endpoints) along with the corresponding database table with audit fields, and used mockaroo to generate simulated data for the government agency persona. I hope to build out a personal dashboard for the government agent where they can view their own funding draft plans, and see ratings on their proposals.

On the frontend, I contributed two full pages for the government agency persona: the Plan Funds dashboard and the Risk Heatmap. The funding dashboard includes a filterable funding index table with filtering by agency and country, a social indicator visual chart with multiselect for indicators and years, and a draft funding plan form that posts to the database. The risk heatmap uses Folium with GeoJSON country boundaries, shading each country by indicator value on a red-to-green color scale, with per-indicator ranking tables which were organized by tabs.

One thing I still want to improve is only showing years where data actually exists for a given indicator, since selecting an unsupported year returns no data. I also added a one-click sync button that fires all six Eurostat POST requests before the page loads data, which was necessary because the database resets on every Docker restart. 

# Other updates
Happy to be home (back from Strasbourg) and feeling like the time in Leuven has flown by so fast! Although this phase was definitely a step up, I feel like it challenged me in unexpected ways, especially in terms of collaboration. Today we went and visited an artisinal chocolatier, learning about the history of chocolate as well as being able to create our own and snack on it while I was coding! I'm looking forward to our debrief next week, more memories, and more building! 
