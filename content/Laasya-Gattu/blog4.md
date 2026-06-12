---
title: "Blog Post 4"
date: 2026-06-11
draft: false
description: "Phase 4 Update"
slug: "blog4"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "laasya-gattu"
showAuthorsBadges : false
---

# Dialogue Phase 4

Since Phase 3, I worked on developing the user interface for the life satisfaction prediction model after it was implemented and hoped to make it more interesting for the user. This included adding more descriptions, a heatmap, and ranked table listing the countries and their corresponding predicted satisfaction score (replacing the single score prediction from earlier). I made a few tweaks to how the model used the user inputs, as initially the model returned a table of countries where every single predicted score was the same. Upon fixing that, the predicted values were still not very interpretable as they were within the range of 7-8.5 for the most part. This was because the initial dataset had values largely within that range, so the model was unable to behave differently to predict among a wider range of scores. I mitigated this by normalizing the scores; setting the minimum score to one and the maximum to 10, which scaled the values shown to the user to be a lot more differentiable (and also worked out better for the heatmap). 

Additionally, I worked on the streamlit design for the home page layouts across the three personas, as well as the student favorites page, where I also added the corresponding favorites table in the SQL script and added the required routes for it. 

Overall, I learned a lot during this project and I think our whole group worked really hard on our MVP and put a lot of effort into making sure it looks like what a real user could genuinely use (after some further developments). I enjoyed the opportunity to learn what the deployed version of ML models could look like in a real-world context and am glad that I got to work on it alongside my peers.

Outside of the project, I enjoyed the activities we did recently, specifically the chocolate factory and the trip to C-Mine. I am glad we got to experience trying the different chocolate and making our own designs While we didn't have much luck exploring Genk, I enjoyed walking through the C-Mine expedition tunnels and seeing the simulations and VR about what it was like to be a miner. I can't believe there's only one day left on the dialogue and I will miss it!