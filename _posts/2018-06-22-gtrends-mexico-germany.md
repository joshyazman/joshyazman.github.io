---
title: "How Google Trends Tell the Story of a World Cup Match"
template: post
---

We tell Google about everything - what our weird rashes look like, which restaurants we eat at, where we need driving directions to get to, and who we want more information about right now. Seth Stephens-Davidowitz writes about using Google and other internet data to answer questions ranging from "How many people in the US identify as LGBT?" to "When do people in Edmonton go to the bathroom during a big hockey game?" His book [Everybody Lies: Big data, new data, and what the internet can tell us about who we really are](https://www.amazon.com/Everybody-Lies-Internet-About-Really/dp/0062390856/ref=sr_1_2?ie=UTF8&qid=1529242271&sr=8-2&keywords=everybody+lies) sparked my interest in using Google Trends data as a low cost market research tool. Here I outline the available data from the `gtrendsR` package - a popular `R` package for accessing Google Trends data - and show how they describe the excitement and buzz around the opening Group F match of the 2018 World Cup between Germany and Mexico. 

# What data is available?
Google Trends data provides an easy, cheap source of information about how salient a topic is on the Internet in a given context. Hits on a search term are measured as an index from 0-100 with 100 representing the highest search volume for a term within some time period. 

For example, in a soccer game, the highest search volume might come right after a big goal or it might, as we'll see with the example of Mexico vs Germany, the highest search volume might come as the game is coming to an end. Google Trends won't tell you that 1.8 million people searched for "Mexico" at around the same time Hirving Lozano scores goal, but we can learn that the highest search interest came right after that goal and the 15th minute saw just a fifth of the search interest of the time of the goal.

The `gtrendsR` R package returns a list of objects related to a search term or set of search terms. Here I've downloaded the latest development version of gtrendsR and loaded the `tidyverse` and `lubridate` packages as well as the mango flavor from the [`LacroixColoR`](https://github.com/johannesbjork/LaCroixColoR) package and some packages related to mapping. I've also loaded a set of demographic data from the US Census Bureau. Finally I downloaded the Google Search interest encompassing searches from the 24 hours leading up to the end of the Mexico vs. Germany World Cup game. The `gtrendsR` package returns this data as a list of objects related to our query.
  
```{r}
library(gtrendsR)
library(tidyverse)
library(lubridate)
mango <- c("#FF5300","#9ED80B","#43B629","#1BB6AF","#8F92A1","#172869")

demos <- read_csv('states_data.csv')%>%
  mutate(pct_hisp = total_hispanic/population,
         pct_mex = mexican/population)
 
mex_ger <- gtrends(keyword = c('Mexico','Germany'), 
                   time = 'now 1-d', 
                   geo = 'US')
```

# Interest over Time
One of the more widely used Google Trends objects is `interest_over_time` data. During the time frame specified, Google returns an index of search interest compared to the highest search volume within that time period. 

Starting with Mexico vs. Germany, Mexico enjoyed higher search interest across the entire game, but search interest spiked in a few [interesting places](https://www.theguardian.com/football/live/2018/jun/17/germany-v-mexico-world-cup-2018-live?page=with:block-5b268946e4b0a90d612a9859#liveblog-navigation). In the lead-up to the game search interest grew as people were searching for details about how to watch. The next notable spike was Hirving Lozano's 35th minute goal to give Mexico an improbable 1-0 lead over Germany. Finally, towards the end of the game, Google searches for Mexico spiked as the game came to a close and Mexico walked away with the win. 

```{r}
game_est <- mex_ger$interest_over_time%>%
  mutate(date = date)%>%#arrange(desc(hits))
  filter(date >= as.POSIXct('2018-06-17 12:54:00'))

ggplot(game_est, aes(x = date, y = hits, color = keyword))+
  geom_line(size = 1)+
  scale_color_manual(values = mango, name = element_blank())+
  labs(title = 'Search Interest by Minute: Mexico vs. Germany',
       x = element_blank(),
       y = 'Hits',
       caption = plot_cap)+
  geom_vline(xintercept = as.POSIXct('2018-06-17 14:01:00')%>%as.numeric(), linetype = 'dashed')+
  annotate('text', x = as.POSIXct('2018-06-17 13:57:00'), y = 50, label = 'Kickoff!', angle = 90, size = 3)+
  geom_vline(xintercept = as.POSIXct('2018-06-17 14:35:00')%>%as.numeric(), linetype = 'dashed')+
  annotate('text', x = as.POSIXct('2018-06-17 14:31:00'), y = 80, label = 'GOOOOOOAL Mexico!', angle = 90, size = 3)+
  geom_vline(xintercept = as.POSIXct('2018-06-17 15:48:00')%>%as.numeric(), linetype = 'dashed')+
  annotate('text', x = as.POSIXct('2018-06-17 15:44:00'), y = 25, label = 'Game winds down', angle = 90, size = 3)
```

<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Search Interest by Minute- Mexico vs. Germany.png#center"/>
</p>

Mexico enjoyed higher levels of search interest across the entire game, but the gap between Mexico searches and Germany searches grew throughout the game. First, around kickoff, there was a little bit more search interest for Mexico than Germany. Then, when Mexico scores the only goal of the game, search interest spiked with significantly more people searching for Mexico than Germany. Towards the end of the game, the bulk of searches mentioned Mexico rather than Germany. 

```{r}
game_est_gap <- mex_ger$interest_over_time%>%
  mutate(date = date)%>%#arrange(desc(hits))
  filter(date >= as.POSIXct('2018-06-17 12:54:00'))%>%
  spread(keyword, hits)

ggplot(game_est_gap, aes(x = date, y = Mexico - Germany))+
  geom_line(size = 1, color = mango[1])+
  # scale_color_manual(values = mango, name = element_blank())+
  labs(title = 'Search Interest Margin by Minute: Mexico vs. Germany',
       x = element_blank(),
       y = 'Difference in Search Interest',
       caption = plot_cap)+
  geom_vline(xintercept = as.POSIXct('2018-06-17 14:01:00')%>%as.numeric(),linetype = 'dashed')+
  annotate('text', x = as.POSIXct('2018-06-17 13:57:00'), y = 15, label = 'Kickoff!',angle = 90, size = 3)+
  geom_vline(xintercept = as.POSIXct('2018-06-17 14:35:00')%>%as.numeric(),linetype = 'dashed')+
  annotate('text', x = as.POSIXct('2018-06-17 14:31:00'), y = 25, label = 'GOOOOOOAL Mexico!',angle = 90, size = 3)+
  geom_vline(xintercept = as.POSIXct('2018-06-17 15:48:00')%>%as.numeric(),linetype = 'dashed')+
  annotate('text', x = as.POSIXct('2018-06-17 15:44:00'), y = 12, label = 'Game winds down',angle = 90, size = 3)
```

<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Search Interest Gap by Minute- Mexico vs. Germany.png#center"/>
</p>

# Related Searches
Google Trends can also tell us about what people are searching for in addition to keywords we specify. The data is presented in several forms:

  * Top Queries - exact search terms people search for in addition to our terms
  * Rising Queries - search terms people search for more during the current time period than the most recent one
  * Top Topics - same as Top Queries, but search topics are curated by Google
  * Rising Topics - same as Rising Queries, but search topics are curated by Google
  
Here we dig into the related queries for both Germany and Mexico, but first clean up the data a bit by getting rid of unhelpful character symbols.

```{r}
rising <- mex_ger$related_queries%>%
  filter(related_queries == 'rising')%>%
  mutate(hits = ifelse(subject == 'Breakout',5000,gsub('\\+|%|,','', subject)))

top <- mex_ger$related_queries%>%
  filter(related_queries == 'top')%>%
  mutate(hits = gsub('<','', subject))
```

## Rising Related Queries
Rising searches return the percentage of growth in search interest comparing our current time interval to the one prior. The time period we're looking at here is the day of the Mexico vs. Germany game and the time interval prior is the day before the game. 
A related query that has grown more than 5,000% since the last time interval is considered to be a "Breakout" query and is coded as such. I've also taken some steps to remove commas, percent signs, and plus signs (but not minus!) from the output for the purpose of plotting search growth as a numeric value. 
The biggest growing search terms mostly relate to how to watch the game by either streaming it online or finding the channel on TV. Interestingly, people searching for "Mexico" seem to have a lot of interest in searching for Hirving Lozano while "Germany" searchers were not nearly as keen to learn more about the man who sent them home with an L. 

```{r}
rising <- mex_ger$related_queries%>%
  filter(related_queries == 'rising')%>%
  mutate(growth = ifelse(subject == 'Breakout',5000,as.numeric(gsub('\\+|%|,','', subject))))

ggplot(rising, aes(x = reorder(value, growth), y = growth, fill = keyword))+
  geom_col(show.legend = FALSE)+
  facet_wrap(~keyword, scales = 'free')+
  coord_flip()+
  theme_minimal()+
  scale_fill_manual(values = mango)+
  labs(title = 'Rising Related Queries Since Prior Day: Mexico vs. Germany',
       x = element_blank(),
       y = 'Search Growth')
```

<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Rising Related Searches.png#center"/>
</p>

## Top Related Queries
Similar to rising related queries, the top related queries returns data on search terms that occur frequently in conjunction with the search term we are analyzing. 
The top related queries in the case of the Mexico-Germany game are mostly variations on "Mexico" and "Germany" along with terms like "world cup" or "soccer."

```{r}
top <- mex_ger$related_queries%>%
  filter(related_queries == 'top')%>%
  mutate(hits = as.numeric(gsub('<','', subject)))

ggplot(top, aes(x = reorder(value, hits), y = hits, fill = keyword))+
  geom_col(show.legend = FALSE)+
  facet_wrap(~keyword, scales = 'free')+
  coord_flip()+
  theme_minimal()+
  scale_fill_manual(values = mango)+
  labs(title = 'Top Related Queries Since Prior Day: Mexico vs. Germany',
       x = element_blank(),
       y = 'Hits')
```
<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Top Related Searches.png#center"/>
</p>

# Interest by Region
The gtrendsR package offers a variety of regional breakdowns of search interest. You may have noticed that search interest for this game is limited to the United States based on the argument `geo = 'US'` in the `gtrends()` function. You can find more geography codes in the `gtrendsR::countries` dataframe.

Interest by geography is offered at the country, region, DMA (media market), and city levels. Here I look at relative search interest by state. Keep in mind this data is presented as an index, so the state with the highest amount of search interest will show 100 hits and the rest are basically a percentage of that state. 

The state with the highest search interest for Mexico is New Mexico - likely in part because of Mexican soccer fans, but also because a wider variety of geographic searches in that state might use the word Mexico (ie: Best dog groomer in New Mexico). Aside from New Mexico, the biggest areas of interest appear concentrated in the Southwest, near Mexico. 

```{r}
library(fiftystater)
library(mapproj)
mexico_regional_interest <- mex_ger$interest_by_region%>%
  filter(keyword == 'Mexico')%>%
  select(state = location, hits)%>%
  mutate(state = tolower(state))

ggplot(mexico_regional_interest,
       aes(map_id = state))+
  geom_map(aes(fill = log(hits)), map = fifty_states, color = 'black')+
  expand_limits(x = fifty_states$long, y = fifty_states$lat) +
  coord_map()+
  scale_x_continuous(breaks = NULL) + 
  scale_y_continuous(breaks = NULL)+
  theme_minimal()+
  fifty_states_inset_boxes()+
  labs(title = 'Game Day Search Interest for "Mexico" by State',
       x = element_blank(),
       y = element_blank(),
       caption = plot_cap)+
  scale_fill_continuous(low = mango[1], high = mango[2], name = 'Search Interest\n(Log Scale)')+
  theme(legend.position = 'bottom')
```

<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/In Game Search Interest for Mexico by State.png#center"/>
</p>

Expanding beyond Google Trends, we can connect the trends data we have with population data from the census and examine the relationship between state level search interest for Mexico and the percentage of people in a state who are of Mexican descent. We can kind of see it on the above map, but the below plot demonstrates a strong positive relationship between the percentage of a state's population that hails from Mexico and the Google search interest from that state. 

```{r}
state_abbrevs <- data.frame(state = c(tolower(state.name),'district of columbia'), state.abb = c(state.abb, 'DC'))
google_census <- inner_join(mexico_regional_interest, 
                            demos%>%mutate(state_con = tolower(state)),
                            by = c('state' = 'state_con'))%>%
  left_join(state_abbrevs)
 
ggplot(google_census, aes(x = log(pct_mex), y = log(hits), label = state.abb))+
  stat_smooth(color = mango[1], size = .25, linetype = 'dashed')+
  geom_text(size = 2.5)+
  labs(title = 'Relationship Between Mexican Pop. Density and Search Interest',
       x = 'Pct of Population from Mexico\n(Log Scale)',
       y = 'Relative Search Interest\n(Log Scale)',
       caption = plot_cap)
```

<p> 
  <img src="https://joshyazman.github.io/images/gtrends-mex-ger/Relationship Between Mexican Pop. Density and Search Interest.png#center"/>
</p>

# Conclusion
The Mexico-Germany game was incredibly fun to watch and a major upset for the Mexican national team. It makes sense that the game generated buzz and the `gtrendsR` package gives us a good way to measure that success. There are some real limitations to using Google Trends for research - Google search is not representative of all soccer viewers, we have to rely on measuring the exact right keywords and trust that users spell things correctly, and the format of the data can be hard to generalize beyond the queries we specifically search for.
