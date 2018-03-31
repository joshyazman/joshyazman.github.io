---
layout: post
title: Using Text Analysis to Figure Out the VA Governor’s Race
date: June 5th 2017
---

# Introduction
On June 13th, Virginia voters will nominate candidates for Governor, Lieutenant Governor, Attorney General, and the House of Delegates. Recently the Washington Post published a [feature](https://www.washingtonpost.com/graphics/local/virginia-governor-race/?utm_term=.f2baf8279dcf) with in depth interviews with the five candidates for governor. This is an interesting data source because newspaper profiles allow candidates to present their preferred image with little filter. We also have a somewhat less rich source of text data from the questions asked by reporters that reflect media interest in each campaign.

Text analysis techniques from [Text Mining with R: A Tidy Approach](https://www.tidytextmining.com/) by Julia Silge and David Robinson allow us to pick apart the words used in questions and responses that most closely align with each candidate.

# Analysis
By measuring term frequency we can determine what each candidate discussed most, but term frequency alone can be problematic. If one candidate talks about education frequently, that’s great. But if every candidate talks about education then education is not unique to any one of them.

Instead, a better metric is term frequency multiplied by inverse document frequency (tf_idf). Essentially, tf_idf measures term frequency but then counts words that most candidates use less than words that only one or two candidates use.

Questions to republicans candidates varied widely. Corey Stewart, a Trump supporter who based a large amount of his campaign on protecting monuments to the Confederacy, received questions emphasizing things like the proposed Trump border wall, confederate imagery, as well as questions about his social and economic agenda. Frank Wagner received a wide variety of questions about his position in the nomination contest and hiseconomic priorities. Ed Gillespie received questions about social issues, healthcare, immigration, and other hot-button topics.

<p>
  <img src="https://joshyazman.github.io/images/text-analysis-vagov-2017/image1.png#center"/>
</p>

Questions to Democrats also varied by candidate. Front-runner Ralph Northam was asked about his support for George W. Bush in his life before elected office. He was also asked about his record on gun safety and the word “angst” indicates he was likely asked about the state of the Democratic party. Tom Perriello received questions about his position on two major natural gas pipelines, his plan for debt free college, and his relationships with Democrats and the legislature.

<p align="center">
  <img src="https://joshyazman.github.io/images/text-analysis-vagov-2017/image2.png#center"/>
</p>

Candidates’ responses also revealed interesting patterns. The Post’s interviews largely gave each candidate an opportunity to present their best arguments for their candidacies, so patterns in their responses are indicative of what they are trying to communicate to voters.

Starting with Republican Corey Stewart, the most important words to his responses note his position in Prince William County. In fact, “Prince” and “William” are by far the two most important words to his responses. Further down the list are words like “heritage” and “plantation” that echo his campaign’s emphasis on the Confederacy. Frank Wagner’s important words almost entirely focus on policies. He is the only Republican candidate to propose raising taxes to pay for infrastructure and his responses indicate a heavy emphasis on transportation and highways. The word “policies” is clearly the most important word in Ed Gillespie’s responses, but actual policy proposals are further down the list. To be fair though, he does emphasize transparency, citizenship, and other policy priorities he has proposed in his campaign.

<p align="center">
  <img src="https://joshyazman.github.io/images/text-analysis-vagov-2017/image3.jpeg#center"/>
</p>

On the Democratic side, Perriello and Northam each got their own policy and tone arguments across distinctively. Aside from repeating the name of his interviewer frequently, Northam emphasized his position on guns, his focus on the November general election, his expertise on medical issues and his experience as a physician. Perriello discussed energy policy, power dynamics in society (as evidenced by references to “power” and “utilities” as well as “consolidation” and “automation). He also used similar language to his interviewer in discussing debt free college.

<p align="center">
  <img src="https://joshyazman.github.io/images/text-analysis-vagov-2017/image4.jpeg#center"/>
</p>

# Conclusion
The Washington Post interviews offered candidates the ability to answer questions as well as define their own reasons for running for Governor. Parsing the text of the questions and answers reveals interesting patterns in the interests of reporters and candidates.
