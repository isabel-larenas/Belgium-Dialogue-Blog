---
title: "Phase 4 Blog Post"
date: 2026-06-11
draft: false
description: "My final blog post!"
slug: "blog4-maira"
tags: ["authors", "config", "docs"]
authors:
  - "maira-padani"
showAuthorsBadges : false
---

# What I Worked On

Phase IV was definitely the most challenging but also the most rewarding. Most of my work was centered around the government agency persona and making the site more user-friendly, though I also worked across the shared API layers and database. The biggest thing I built was the government agent persona pages, including the `funding_draft` table in `01housing_db.sql`, the funding draft routes in `funding.py`, and three pages for exploring existing EU funding programs, creating new draft proposals, and managing your own drafts. I also added success banners for save and delete actions so users get clear feedback. In the beginning, all of our routes were in a single file with a single blueprint. I split them out so each entity has its own blueprint — `housing.py`, `listing.py`, `reviews.py`, `funding.py`, `stats.py`, and `prediction.py` then updated `rest_entry.py` to register each one, making the repo much easier to navigate. I also built the risk heatmap, which became the visual model for the deprivation map on the government predictor page. It is a Folium choropleth map with year and indicator filters and a side-by-side bar chart. One tricky part was handling country name mismatches between the GeoJSON source and our database — for example "Czech Republic" vs "Czechia." I added download buttons for the map as HTML and the rankings as CSV. I also added the social indicator routes and fixed the HPI sync, wired the student model to the frontend with a `/student/prediction` route, updated all three persona home pages with a consistent layout and the EuroHome animation, and cleaned up the repo including rewriting the README files. Overall this phase pushed me to think more deeply about how the frontend and backend connect, and how design decisions in one layer can really affect the other. I really enjoyed this project and the collaborativeness I had with my team. We all really came together and put our own pieces and features in to create a fully functioning useful app that hopefully would help someone navigate the EU housing market. 

# Other Updates
We had many amazing excursions in the past week and a half. My favorite visit this week was the C-Mine! It was so cool to see the history behind Genk and the coal mining community. I enjoyed the VR component, climing up almost 300 steps for an amazing view off of a mine shaft tower and the maze. The other exciting activity we had was the escape room! My team crushed it, except for the fact when we didn't actually beat it and get out. Nonetheless, it was a learning experience. Lastly, the chocolate experience was fantastic. I enjoyed being a artisan chocolate maker, making a chocolate Seamus, and the amazing outfits we all got to wear. I'm sad to say there's only a couple days left in the dialogue, but this program was such a fulfilling, learning experience with some amazing people. 


<img src="/Belgium-Dialogue-Blog/images/pasta.png" style="max-width: none !important; width: 500px !important;">

<img src="/Belgium-Dialogue-Blog/images/mines.png" style="max-width: none !important; width: 500px !important;">

<img src="/Belgium-Dialogue-Blog/images/antwerp.png" style="max-width: none !important; width: 500px !important;">

