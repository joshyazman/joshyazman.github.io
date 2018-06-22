---
title: "How Google Trends Tell the Story of a World Cup Match"
template: post
---

We tell Google about everything - what our weird rashes look like, which restaurants we eat at, where we need driving directions to get to, and who we want more information about right now. Seth Stephens-Davidowitz writes about using Google and other internet data to answer questions ranging from "How many people in the US identify as LGBT?" to "When do people in Edmonton go to the bathroom during a big hockey game?" His book [Everybody Lies: Big data, new data, and what teh internet can tell us about who we really are](https://www.amazon.com/Everybody-Lies-Internet-About-Really/dp/0062390856/ref=sr_1_2?ie=UTF8&qid=1529242271&sr=8-2&keywords=everybody+lies) sparked my interest in using Google Trends data as a low cost market research tool. Here I outline the available data from the `gtrendsR` package - a popular `R` package for accessing Google Trends data - and show how they describe the excitement and buzz around the opening Group F match of the 2018 World Cup between Germany and Mexico. 

# What data is available?
Google Trends data provides an easy, cheap source of information about how salient a topic is on the Internet in a given context. Hits on a search term are measured as an index from 0-100 with 100 representing the highest search volume for a term within some time period. 

For example, in a soccer game, the highest search volume will likely come right after a big goal. Google Trends won't tell you that 1.8 million people searched for "Mexico" at around the same time Chicharrito Hernandez scores goal, but we can learn that the highest search interest came right after that goal and the 15th minute saw just a fifth of the search interest of the time of the goal.

The `gtrendsR` package returns a list of objects related to a search term or set of search terms. Here I've downloaded the latest development version of gtrendsR and loaded the `tidyverse` and `lubridate` packages as well as the mango flavor from the [`LacroixColoR`](https://github.com/johannesbjork/LaCroixColoR) package. I've also loaded a set of demographic data from the US Census Bureau and loaded the Google Searches for the day encompasing the Mexico - Germany match in two ways:

  * First I downloaded the prior day's search interest shortly after the game. Google gives us this data with a fair amount of granularity. Search interest over time is measured in 8 minute increments.
  * Second, I set up a loop to download the past hour's search interest every 3 minutes. When we search for trends in the past hour we get minute by minute data.
  
```{r}
library(gtrendsR)
library(tidyverse)
library(lubridate)
mango <- c("#FF5300","#9ED80B","#43B629","#1BB6AF","#8F92A1","#172869")
theme_set(theme_minimal())
demos <- read_csv('states_data.csv')%>%
  mutate(pct_hisp = total_hispanic/population,
         pct_mex = mexican/population)
 
mex_ger <- gtrends(keyword = c('Mexico','Germany'), 
                   time = 'now 1-d', 
                   geo = 'US')

plot_cap <- 'Source: Google Trends\nJosh Yazman (@jyazman2012)'
```

<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/In Game Search Interest for Mexico by State.png#center"/>
</p>
<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Relationship Between Mexican Pop. Density and Search Interest.png#center"/>
</p>
<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Rising Related Searches.png#center"/>
</p>
<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Search Interest Gap by Minute- Mexico vs. Germany.png#center"/>
</p>
<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Search Interest by Minute- Mexico vs. Germany.png#center"/>
</p>
<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Top Related Searches.png#center"/>
</p>
