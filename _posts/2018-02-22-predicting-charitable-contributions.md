---
layout: post
title: Predicting Charitable Contributions with Gradient Tree Boosting
---
Excessive overhead spending has long been seen as a cardinal sin for charitable organizations. Money spent on corporate salaries and donor outreach is a key part of how charities are evaluated by organizations like [Charity Navigator](https://www.charitynavigator.org/index.cfm?bay=content.view&cpid=1284) who signal to donors that certain organization are worthy of their dollars. One way to reduce overhead is to boost the efficiency of donor outreach by limiting outreach to those most likely to donate to your organization. This post demonstrates the use of tree-based methods for predicting donor likelihood for a fictional charity ([code](https://github.com/joshyazman/medium-posts/tree/master/predicting-charitable-donations)).

# The Data
The data provided contains 6,003 records on donors that a fictional charity provided for a class in Northwestern University’s [MS. in Predictive Analytics](https://sps.northwestern.edu/masters/data-science/) program. The goal of the project was to suggest a list of potential donors to receive mail solicitations that would maximize profit for the charity assuming a cost of $2 per letter and average donation amount of $14.50.

# Growing a Tree
Tree-based methods use a process called greedy recursive binary splitting. Essentially, we find smart ways to divide up our data into groups and then divide those groups into groups. After a few rounds of this process, the average of each group (terminal node) is considered the predicted value for all objects in the group. In the context of donation likelihood, the percentage of donors in each terminal node is considered the predicted donation likelihood for every person in that node.

# Now Let’s Grow a Lot of Trees!
Building a single tree may find the best ways to split up your data set today, but risks failing when you try to apply the model to new data. To mitigate that risk, we grow thousands of trees with slightly different samples of data and average all of the model predictions for each person. The average of 10,000 estimates is likely to be more generalizeable and just as accurate as a single estimate.

# Evaluating the Model
There are two things we need to know about the classification model. We obviously want to know how accurately this method classifies potential donors, but we also want to understand how the models work. First, we look at which variables were most important in determining predictions. Then we take a broader look at model accuracy.

## Variable Importance
To understand how each tree grew its terminal nodes, we examine the number of times each variable was used to divide up the data set out of the 10,000 trees grown.

<p>
  <img src="https://joshyazman.github.io/images/charitable-predictions/image1.png#center"/>
</p>

The model heavily emphasized number of children in the household as well as household income, wealth rating, and home ownership. It may be a bit surprising to see region2 included, but maybe the fictitious charity is in that part of the country and has a strong local presence.

## Model Validation
Most of the people with high donation probabilities did, in fact, donate and most people with low likelihoods did not donate. Ideally the distribution of donation predictions would be a little bit more separated, but this looks pretty good! Using a cutoff of 50% donation likelihood, we correctly classify 87% of donors and non-donors.

<p>
  <img src="https://joshyazman.github.io/images/charitable-predictions/image2.png#center"/>
</p>

To measure profit lift, recall that the average donor gives $14.50 and each letter costs $2.00 to send. Mailing to the entire list returns $31,409 in profit. But given the profit margin of each successfully solicited donor, only mailing to those with 50% donation likelihood or higher would leave a lot of money on the table.

<p>
  <img src="https://joshyazman.github.io/images/charitable-predictions/image3.png#center"/>
</p>

Optimal use of this model means mailing to everyone with a likely donor score above 35% which yields $35,294 in profit. When using the optimal cutoff, our fictional charity boosts profits by 12.4%.

# Conclusion
Charitable organizations seek to maximize donations (to fund their missions as fully as possible) while minimizing the amount of resources devoted to fundraising. The tree-based method presented here increased the donation rate from one mailer from 49% to 87% and boosted overall profits by 12% almost exclusively by reducing the number of mailers sent.

# One Last Thing
If you’ve read this far and want to donate to a charity yourself, I encourage you to support the Pancreatic Cancer Action Network in honor of my mom, who you can read more about [here](https://www.pancan.org/about-us/pancreas-matters-enewsletters/wage-hope-sons-story/). Donate at [pancan.org/donate](pancan.org/donate).

<p>
  <img src="https://joshyazman.github.io/images/charitable-predictions/image4.png#center"/>
</p>
