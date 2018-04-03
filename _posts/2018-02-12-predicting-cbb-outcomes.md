---
layout: post
title: Predicting College Basketball Outcomes
---
The surest strategy to win a college basketball game is to score more points than the other team. Or, as John Madden more artfully stated: “You can’t win a game if you don’t score any points.”

So when it comes to modeling win likelihood it stands to reason that the easiest way to predict who will win is to predict who will score more points. The Las Vegas point spreads tend to be very good predictors of score differential (Harville 1980) and those point spreads can be combined with efficiency metrics like points per hundred possessions to create strong classifiers (Lopez and Matthews 2014).

But sometimes point spreads are not available, like when putting together a bracket for the NCAA tournament, which requires outcome prediction prior to publication of specific game odds.

This paper seeks to find a reliable two-step process for predicting outcomes without relying on outside expert ratings or point spread predictions. First, develop a reliable regression method for predicting point spreads. Then use point spread predictions as an input into a classification model (Lopez and Matthews 2014). Several methods are attempted for each step and evaluated for predictive accuracy on a hold-out dataset¹.

A final innovation of this paper is the complete reliance on data that would be openly available prior to game play².

# Data
The models presented are built and tested using game log data for 351 Division 1 men’s basketball teams downloaded from sports-reference.com. Game logs for each team provide one row of data per game per team including various statistics such as points for and points against as well as steals, blocked shots, and three pointers made and attempted. Training data consists of game logs from 2014–2017 and models are tested on 2017–18 game logs — a typical sample size for projects of this nature (Gupta 2014).

Similar to the data structure used by Yuan et al (2014), the data is formatted to reflect information available prior to game play. For each game, all statistics are represented as an average or sum of all games prior within that season. For example, one input value for the 12th game of Villanova’s 2017–2018 season would be the average point spread from their first 11 games. Addressing concerns about matchup effects (Hoegh et al 2014) statistics like points for and points against are consolidated to spreads with the formula `points for — points against`. All data is standardized prior to model development.

Lastly, since team performance varies significantly early in the season ([Yazman 2017](https://joshyazman.github.io/when-can-i-rely-on-basketball-stats/)), only games after the 12th game of the season are used to develop prediction models and the models presented here should only be applied to mid-late season games.

# Regression Methods
Several linear regression techniques are attempted in order to predict point spreads prior to game play. Parametric methods include linear regression with heuristic, LASSO, and least angle variable selection techniques. Non-parametric methods include boosted regression trees and random forest models.

## Heuristic Variable Selection
The first model attempted is a linear model using predictors selected from a combination of domain expertise and exploratory analysis. Initial correlations, presented in Figure 1, suggest that very few predictors show strong correlations with point spread, but the top four variables are the average spread from prior games, field goal percentage, assist spreads, and defensive rebounding spreads.

<p> 
  <img src="https://joshyazman.github.io/images//predicting-cbb-outcomes/image1.png#center"/>
</p>

The data meets some assumptions of linear regression in that errors are mostly normally distributed with a mean close to zero. However, predicted values are generally more conservative than real outcomes (Figure 7) and errors tend to be highly correlated with point spread (Figure 2).

<p> 
  <img src="https://joshyazman.github.io/images//predicting-cbb-outcomes/image2.png#center"/>
</p>

Additionally, several of the predictor variables are highly correlated with one another.

## LASSO Regression
A possible answer to the multicollinearity concerns evident in the heuristic approach is to employ regularized regression methods like the LASSO. Rather than making binary choices to include or exclude certain variables, these methods consider all possible predictors and weight their value to the appropriate scale for the data.

The LASSO method starts with all possible predictors and shrinks coefficients towards zero based on assigned constraints (James et al 2017). The constraint, lambda, reduces coefficient sizes as its value increases. The optimal lambda, as identified through cross-validated trial and error across a number of potential values is 0.0103 (Figure 3).

<p> 
  <img src="https://joshyazman.github.io/images//predicting-cbb-outcomes/image3.png#center"/>
</p>

Whereas in the heuristic process, only four variables are used, LASSO regression uses almost all possible predictors with varying weights. Only 8 variables out of 35 features in the data are completely excluded. Playing on one’s home court has significantly more weight than most variables while defensive rebounding spread — a variable included in the heuristic selection model, is excluded from the model. The full set of regression coefficients is presented in Figure 4.

<p> 
  <img src="https://joshyazman.github.io/images//predicting-cbb-outcomes/image4.png#center"/>
</p>

## Least Angle Regression (LAR)
For this method, a model is initiated using the variable with the highest correlation with the target (point spread). Then the coefficient for the predictor variable is adjusted until another variable has a higher correlation than it does, at which point the first coefficient is locked into the motions of the next coefficient and the process is repeated until all predictors have had a chance for inclusion. The process adds variables iteratively, as would be done in a forward step-wise approach, but “only enters ‘as much’ of a predictor as it deserves” (Hastie et al 2001). The full set of LAR coefficients is presented in Figure 5.

<p> 
  <img src="https://joshyazman.github.io/images//predicting-cbb-outcomes/image5.png#center"/>
</p>

## Boosted Regression Trees
The boosted regression tree method employs a process called greedy recursive binary splitting to divide values into subgroups, or terminal nodes, that share similar attributes. Then the predicted value for point spread will be the average point spread for all observations within the group. Greedy recursive binary splitting is greedy because each step splits the data in the best way possible at that point in the process (James et al 2017). The split is determined to be combination of predictor and split point that minimizes error. Each terminal node will individually exhibit high amounts of bias, but by resampling the data and repeating the process several thousand times, we smooth out the errors and return reliable predictions. The model applied here uses 10,000 trees at a maximum node depth of 6 layers.

## Random Forest
Random Forest models work similarly to boosted regression trees, but instead of limiting tree depth, we build fewer deeper trees. In this case we build 500 full trees.

# Evaluating Regression Models
Unfortunately, no one regression model provided accurate enough point spread estimates to make an obvious impact on classification modeling. All model predictions generally share a positive relationship with observed values (Figure 6), but their individual distributions are tighter than the observed values and contain far too many zero values, which should not exist because there are no ties in basketball (Figure 7).

<p> 
  <img src="https://joshyazman.github.io/images//predicting-cbb-outcomes/image6.png#center"/>
</p>

But accuracy in point spreads as a goal is secondary to classification accuracy. Figures 8 and 9 illustrate the relative strengths of each model in terms of classification. None of the models have hugely different distributions for winning and losing teams (Figure 8), but Random Forest may eke out an advantage as an input for future classification models (Figure 9).

<p> 
  <img src="https://joshyazman.github.io/images//predicting-cbb-outcomes/image7.png#center"/>
</p>

Additionally, the random forest point spread appears to discriminate between wins and losses better than most naturally appearing variables. The next step is to build a classification model using the Random Forest prediction as an input.

# Classification
Classification differs in mission from regression in the sense that rather than predicting point spreads, the goal is to assign a probability of one team winning over the other. The classification model development techniques are similar to those employed in the point spread regression context — LASSO, Boosted Classification Tree, and Random Forest.

<p> 
  <img src="https://joshyazman.github.io/images//predicting-cbb-outcomes/image8.png#center"/>
</p>

The random forest and boosted tree models clearly outperform LASSO logistic regression, which clusters predictions too close to 50/50 to be terribly useful.

<p> 
  <img src="https://joshyazman.github.io/images//predicting-cbb-outcomes/image9.png#center"/>
</p>

Setting a classification cutoff of 50% (ie: predicted win probabilities over .5 evaluate to a predicted win, otherwise the prediction is a loss), the Lasso has a slightly higher Type I error rate and slightly lower Type II error rate than either Boosted Trees or Random Forest, which each give similar outputs.

# Conclusion
In both regression and classification contexts, tree-based methods beat out shrinkage and heuristic methods. The random forest method outperforms all other methods (albeit by a slim margin) in point spread predictions. Both boosted classification tree and random forest methods outperform Lasso regression in predicting win probabilities.

Since betting against the spread can be more lucrative than betting against the money line, one option to predict point spreads could involve applying the fairly good classification models to the best point spread prediction methods. Future research should explore the possibility of iteratively repeating back and forth this process until predictive power diminishes or some threshold is met.

# Sources
David Harville, Journal of the American Statistical Association, Vol. 75, №371 (Sep., 1980), pp. 516–524
Lopez and Matthews, Journal of Quantitative Analysis in Sports, Volume 11, Issue 1
Yuan et al, Journal of Quantitative Analysis in Sports, Volume 11, Issue 1
Gupta, Journal of Quantitative Analysis in Sports, Volume 11, Issue 1
Hoegh et al, Journal of Quantitative Analysis in Sports, Volume 11, Issue 1
T. Hastie, R. Tibshirani, and J. Friedman. Springer Series in Statistics Springer New York Inc., New York, NY, USA, (2001)
G. James, D. Witten, T. Hastie, R. Tibshirani. Springer Series in Statistics Springer New York Inc., New York, NY, USA (2017)
¹ All code and data can be found [here](https://github.com/joshyazman/northwestern/tree/master/predict-454/basketball-prediction).
² Special thanks for Guy Molyneux for suggesting this constraint.
