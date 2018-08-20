---
layout: post
title: Reverse Coattails in the VA Governor’s Race
---
One of the more controversial hypotheses coming out of the 2017 statewide elections in Virginia is the idea of reverse coattails — a phenomenon where House of Delegates candidates boosted the vote share for the top of the ticket. Most writing on this topic has focused on whether or not Ralph Northam got a greater share of the vote in precincts with contested house races. I tend to agree with previous work that argues that down-ballot competition doesn’t really affect support for the top of the ticket. But elections are about vote share and turnout. Perhaps down-ballot competition increases turnout in helpful ways for statewide candidates.

Data collected from the State Board of Elections allows for precinct-level analysis of of voter turnout and support. In order to match election results with precinct-level registration data, absentee and provisional votes are not included in this analysis.

For this analysis, I compare turnout and support in the Governor’s race to turnout and support in house races based on whether or not they were contested. Then we simulate the results of the Governor’s race if support remained static, but turnout had not been influenced by competition down ballot.

# Competition Drove Turnout
There is clear evidence that turnout for the Governor’s race was higher in precincts with a contested house race. This makes intuitive sense because campaign efforts at the statewide level are supplemented by efforts from local candidates and their campaigns. It’s one thing to have a volunteer to canvass for you once per week in a precinct, but it’s even better to have a candidate in an area devoting all of their time, energy, and resources to turning out votes.

Figure 1a illustrates the difference in top-of-ticket turnout when races are contested as opposed to when races are not contested. Turnout was demonstrably higher at the top of the ticket in contested precincts, but that’s not just because more people ran for office in places that were more excited about turning out to vote.

<p> 
  <img src="https://joshyazman.github.io/images/reverse-coattails-vagov2017/image1.png#center"/>
</p>

Figure 1b displays the same distribution for House of Delegates races. Down-ballot roll-off (where voters show up for the top of the ticket but abstain from voting in local races) was much more severe in places without a contested house race, which suggests house races were turning out voters that wouldn’t necessarily turn out in other districts.

<p> 
  <img src="https://joshyazman.github.io/images/reverse-coattails-vagov2017/image2.png#center"/>
</p>

# What Would Happen Without HoD Races?
It’s hard to accurately simulate this kind of counterfactual, but one approach would be to use the existence of a competitive race as an independent variable in a linear model of turnout. The resulting coefficient tells us the marginal impact of competition on turnout is 3.5%! That means turnout is 3.5% higher in places with a house race than it would be in any off-year old statewide election². That’s an enormous turnout benefit for the party. But was it a game changer for Governor-elect Northam?

Probably not. In fact, if we hold vote share constant and reduce turnout by 3.5% across all precincts with a competitive house race, Northam’s margin actually grows by about 1,400 votes.

# Conclusion
Does this mean we shouldn’t bother to run candidates all over the commonwealth? Absolutely not. Even if the reverse coattails effect is minimal for the top of the ticket, Democrats won 15 (maybe 16!) seats in the House of Delegates, virtually erasing the overwhelming Republican majority that had been a fixture in Virginia politics for years. Many of the winners in these house races came from districts that wouldn’t have appeared on many lists of targeted races a year ago. By running everywhere and driving up turnout in competitive precincts, Democratic house candidates may not have boosted Ralph Northam’s prospects, but they did demonstrate many other benefits to running candidates across the commonwealth.

¹Code for all above data collection and presentation is [here](https://github.com/joshyazman/medium-posts/tree/master/reverse-coattails-2017hod).

² This estimate is almost certainly too high in real life, but for the sake of dispelling the idea of reverse coattails, overestimating the turnout effect of downballot competition without finding a corresponding advantage for the top of the ticket only bolsters the argument that reverse coattails were not a factor in the governor’s race.
