---
layout: post
title: When Can I Start Relying on Basketball Stats?
---

One challenge in predicting basketball outcomes is the unpredictability inherent in college sports. Players get injured or suspended, teams have great nights and off nights, and particularly at the beginning of the season not all match-ups are on the same competitive level.

The law of large numbers indicates that after a certain number of games played, the average of any statistic should trend towards the true average. However, in order to maximize the number of accurate predictions for the season, predictions need to be made as early as it is possible to obtain good, steady projections of predictor variables. The purpose of this post is to determine how many games into the season a forecaster needs to wait in order to make good, stable predictions without sitting out too much of the season.

# Obtaining and Preparing the Data
The available data is downloaded from sports-reference.com. Each college basketball team has a page containing a `.csv` file of their game logs for the season. Game logs are downloaded for 351 teams between the 2012–2013 and 2016–2017 seasons ([example](https://www.sports-reference.com/cbb/schools/virginia-tech/2017-gamelogs.html)). All regular season and post-season games are included. Fields such as points for and points against are condensed to differentials (for each variable we use `n_for — n_against`). The data is then scaled and centered as it will be for the actual model building process. Lastly, missing data is imputed using the `mice` package in `R`.

# When does the variability level off?
The next step is to loop through each combination of season, team, and potential starting point in the season to calculate the mean and median values for each variable. Values are repeatedly calculated on sub-samples of the data to create a bootstrapped distributions.

The standard deviations of mean and median distributions for each team and season are plotted against the number of games played before the statistic was calculated.

<p> 
  <img src="https://joshyazman.github.io/images/when-can-i-rely-on-bball-stats/image1.png#center"/>
</p>

# Conclusion
It looks like the variability in all of these metrics tends to level off somewhere around 12–13 games into the season. This is going to be helpful to know for developing forecasts, which will be a topic for a future post. But the results here also demonstrate that you can feasibly predict outcomes despite exogenous factors that impact all teams.
