---
layout: post
title: Sentiment Analysis from the Second Presidential Debate
---

# Introduction
After reading [this](http://varianceexplained.org/r/trump-tweets/) article by David Robinson demonstrating that Donald Trump’s most vitriolic and nasty tweets were coming from his Android while more traditional campaign tweets came from staffers using an iPhone, I’ve been itching for a good excuse to apply Robinson’s sentiment analysis on my own. The second presidential debate offered just such opportunity. All of my code and data are here if you want to follow along at home.

# Who Spoke More?
One of the immediate arguments after each debate is who got to speak more. Luckily just about every media outlet tracked this and determined that speaking time was almost identical with a slight [edge for Trump](https://www.politico.com/story/2016/10/2016-presidential-debate-speaking-times-229502).

Another way of looking at speaking time is to look at the length of each response. The average word count (illustrated by the diamond on the plot below) was much higher for Clinton than Trump because his responses tended to be short and frequent.

[Violin Plot](joshyazman.github.io/images/second-debate-plots/second-debate-image1.png)

The low word-count responses are probably pumped up by Trump’s 44 interruptions (like response 18: “I’d like to respond to that. I assume I can.”) which were each coded as separate responses. Clinton’s responses were more evenly distributed across a variety of word counts, likely because her remarks tend to be prepared and measured and she interrupted far less often than Trump.

# What Sentiments Did Candidates Express Most?
Next I matched the words in the transcript to the [NRC emotion lexicon](http://www.aclweb.org/anthology/W10-0204) to determine sentiments of candidate responses. Overall, Clinton was much more positive than Trump and tended to use words that evoke trust, joy, and anticipation more often. Conversely, Trump was more negative and used words that evoke surprise, sadness, fear, disgust, and anger.

![Sentiment bar chart](joshyazman.github.io/images/second-debate-plots/second-debate-image2.png)

But at my debate watch party, attendees didn’t seem to walk away feeling the way the aggregate sentiments might suggest. Perhaps that’s because Clinton started off more positive and finished the debate on a more negative note (and opposite for Trump) as the following series of charts suggests.

![Sentiments Time](joshyazman.github.io/images/second-debate-plots/second-debate-image3.png)

# Why Does This Matter?
The tone of the debate, much like the rest of the campaign this year, was not particularly uplifting. What this data demonstrates is that the negativity isn’t evenly distributed across both candidates. While Trump ended on a positive note, he also admitted to tax evasion, riffed on how Muslims don’t do enough to keep us safe, and offered a roundly panned response for the video in which he brags about getting away with sexual assault. Clinton certainly attacked Trump (on many of those very comments) but her overall tone was much more positive than his.

I’ll be watching the third debate and maybe posting a similar article about, but let’s be real — we’re all just waiting to hear the acceptance/concession speeches and be done with it all.
