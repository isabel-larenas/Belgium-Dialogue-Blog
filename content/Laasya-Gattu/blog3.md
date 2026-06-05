---
title: "Blog Post 3"
date: 2026-06-05
draft: false
description: "Phase 3 Update"
slug: "blog3"   # if you use, needs to be different for every post
tags: ["authors", "config", "docs"]
authors:
  - "laasya-gattu"
showAuthorsBadges : false
---

## Dialogue Phase 3
For Phase 3 of the project, I contributed to updating the previous POC ML model with the phase 2 feedback. Initially, we created a KNN model to provide a recommender system to our student user persona, but ultimately we decided to update the model to a linear regression to provide a more personalized prediction. Isabel and I found a new dataset that provides information on [overall life satisfaction by age, sex, and education](https://ec.europa.eu/eurostat/databrowser/view/ilc_pw01$dv_2761/default/table?lang=en&category=qol.qol_lif.qol_life_sat), so I worked on redeveloping the model to use the crime, pollution, noise, and degreee of urbanization features to predict the overall satisfaction level (1-10 rating). I cleaned and merged the new dataset, updated the plots, and modified the linear regression model overall. I made the python script with the train, test, and predict functions as well so the routes and streamlit could be implemented. I also added the streamlit page for the student persona's Housing Satisfaction Predictor page. Some of the features included are the sliders for user input, which outputs the prediction metrics. The net steps for the streamlit would be to add some more metrics for the student user persona to view and navigate through, as well as adding the features to save listings. I also will be implementing the plots to be interactive on the government agency manager's streamlit pages next.

Outside of the project, I enjoyed the trip to Luxembourg and listening to the speakers at Eurostat, as it was very relevant to the data collection we conducted for our project. I also really liked the visit to the chocolate factory, and it was a nice break from the academic activities.


