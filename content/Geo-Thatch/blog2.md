---
title: "Phase 2 Blog Post"
date: 2026-05-26
draft: false
description: "What I worked on in phase 2!"
slug: "blog2"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "geo thatch"
showAuthorsBadges : false
---
# What I worked on
For phase II of the project, I worked on the ER diagrams for the Government project manager, Real estate agent, and the Global data model. After finishing that, I created the DDL model based on the attributes and relationships previously shown in the ER model. From there, I wrote the SQL code and put it into datagrip. For the most part, Maira and I worked together for the ER models and shared a lot of the work throughout. The wireframes were largely based off of our in class work, during which I worked on the real estate agent’s pages, though we did revise them later. Most of my time was spent with the local ER models, because we were initially struggling to figure out exactly how to store the statistics (pollution, crime, house price index, etc.). We considered creating an entity for each different statistic, but after speaking with Dr. Fontenot, we reconsidered and ended with our current structure, having social_indicator_stats and social_indicator_type. This way, all of the numbers and data will be stored in the stats entity, but the type will be specified with the SIT entity. In practice, we will likely be using a JOIN statement to connect these two tables and have the data for any given operation or visualization. 

# What I saw
This week I enjoyed going to Bruges, Brussels, and bowling! In Bruges, I went on a boat tour around the city, and it was very interesting to see the older architecture. I’ve attached some photos below. 


![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/atomium.png)
![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/boat.png)
![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/bowling.png)
![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/food1.png)
![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/food2.png)
![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/pretty1.png)
![image](https://isabel-larenas.github.io/Belgium-Dialogue-Blog/images/pretty2.png)
