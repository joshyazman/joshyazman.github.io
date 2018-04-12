---
layout: post
title: Examining FiveThirtyEight’s Soccer Power Index Ratings
date: June 5th 2017
---

FIveThirtyEight recently released their newest batch of soccer power index (SPI) ratings for over 400 soccer teams around the world. I don’t know much about soccer, but I love data and enjoy the occasional game when it’s on, so I copied the scores into an Excel file, cleaned up the data a bit and loaded it into R to see what I could learn! Code for preparing the data and visuals is included below.

# Preparing the data
First, I read the data into R. Then I looped through each unique league and calculated the pairwise difference in SPI for each league. Finally, I combined all of the individual league data frames into one.

`
# Make this your working directory
setwd('your/working-directory/here')
# yaz_theme.r loads the dplyr and ggplot packages as well as a formatting function for my own ggplot theme and color scheme
source('yaz_theme.r')
#Read the file
scores <- read.csv('yourfile.csv')
# League is calculated to avoid problems with leagues in different countries that have the same name
scores <- scores%>%mutate(league = paste0(COUNTRY,' - ',LEAGUE))
# Compute pairwise differences in SPI by league and then combine them
pair.dfs <- list()
leagues <- unique(scores$league)[-24]
for(i in seq(1, length(leagues))){
  df <- scores%>%filter(league == leagues[i])
  df.paired <- expand.grid(df$TEAM, df$TEAM)%>%
    select(team.a = Var1, team.b = Var2)
  df.paired.a <- left_join(df.paired, df%>%
                             dplyr::select(TEAM, league, SPI), by = c('team.a'='TEAM'))%>%
    select(team.a, team.b, league, spi.a = SPI)
  df.pair.both <- df.paired.a%>%
    left_join(df%>%
                select(TEAM, spi.b = SPI), by = c('team.b'='TEAM'))%>%
    select(team.a, team.b, league, spi.a, spi.b)%>%
    mutate(spi_diff = abs(spi.a - spi.b))
  pair.dfs[[i]] <- df.pair.both
}
pairs <- bind_rows(pair.dfs)%>%
  filter(team.a != team.b)
`

# Overall Competitiveness
The first metric I want to look at is overall competitiveness of each league by examining distributions of pair-wise differences in SPI. To visualize this, I used a ridgeline plot from the ggridges package which stacks distribution plots broken out by category on top of one another. Major League Soccer in the US appears to have a normal distribution where most teams have similar SPIs and just a few teams are really terrible or really great. On the contrary, Italy’s Serie A and the British Premier League have much broader distributions of scores. There are some really bad teams and some fantastic teams! Overall, almost every league appears to center around fairly small pairwise SPI differences.

```
library(ggridges)
library(gridExtra)
# Create the competitiveness joy plot
comp <- ggplot(pairs, aes(x = spi_diff, y = league))+
  geom_density_ridges(fill = yaz_cols[4], alpha = .5)+
  labs(x = 'Distribution of SPI Differences',
       y = element_blank(),
       title = 'Competitiveness by League',
       caption = 'Source: projects.fivethirtyeight.com/global-club-soccer-rankings/')+
  theme_yaz(base_size = 10)+
  theme(axis.text.y = element_blank())
# Create the SPI by League joy plot
overall <- ggplot(pairs, aes(x = spi.a, y = league))+
  geom_density_ridges(fill = yaz_cols[3], alpha = .5)+
  labs(x = 'Soccer Power Index',
       y = element_blank(),
       title = 'SPI by League',
       caption = 'Source: projects.fivethirtyeight.com/global-club-soccer-rankings/')+
  theme_yaz(base_size = 10)
# Print both side by side
grid.arrange(overall, comp, nrow = 1)
```

<p>
  <img src="https://joshyazman.github.io/images/spi-ratings-2017/image1.png#center"/>
</p>

# Deep Dive into a Few Leagues
The US’s MLS and England’s Premier League stand out as examples of interesting distributions along each score. Next we can use heatmaps (geom_tile) to dive deep into these leagues and see which teams are rising above the rest.

## British Premier League
Examining the differences in SPI among British teams confirms the polarization evident in the above joy plots. About six teams (Manchester United through Liverpool) are competitive with one another. The rest of the league teams are competitive with each other. But the top six teams are far removed from the rest of the pack.

```
ggplot(pairs%>%filter(team.a != team.b & league == 'England - Premier League'), 
       aes(reorder(team.a, desc(spi.a)), reorder(team.b, desc(spi.b)), fill = spi_diff))+
  geom_tile()+
  theme_yaz(base_size = 10)+
  scale_fill_continuous(name = 'Difference\nin SPI', low = yaz_cols[3], high = yaz_cols[4])+
  theme(axis.text.x = element_text(angle = 90),
        legend.position = 'right', legend.direction = 'vertical')+
  labs(title = 'Difference in SPI by Team-Pair',
       x = element_blank(),
       y = element_blank(),
       subtitle = 'SPI Calculated by FiveThirtyEight for 20 BPL Teams',
       caption = 'projects.fivethirtyeight.com/global-club-soccer-rankings/')
```

<p align="center">
  <img src="https://joshyazman.github.io/images/spi-ratings-2017/image2.png#center"/>
</p>

## US — Major League Soccer
MLS teams are much less polarized. The bulk of the heatmap is greenish — indicating that most teams aren’t that different from one another in terms of SPI. If there is a standout team, it’s Toronto FC.

```
ggplot(pairs%>%filter(team.a != team.b & league == 'USA - Major League Soccer'), 
       aes(reorder(team.a, desc(spi.a)), reorder(team.b, desc(spi.b)), fill = spi_diff))+
  geom_tile()+
  theme_yaz(base_size = 10)+
  scale_fill_continuous(name = 'Difference\nin SPI', low = yaz_cols[3], high = yaz_cols[4])+
  theme(axis.text.x = element_text(angle = 90),
        legend.position = 'right', legend.direction = 'vertical')+
  labs(title = 'Difference in SPI by Team-Pair',
       x = element_blank(),
       y = element_blank(),
       subtitle = 'SPI Calculated by FiveThirtyEight for MLS Teams',
       caption = 'projects.fivethirtyeight.com/global-club-soccer-rankings/')
```

<p align="center">
  <img src="https://joshyazman.github.io/images/spi-ratings-2017/image3.png#center"/>
</p>

# Conclusion
If you’re deciding which leagues might be most exciting to watch, one way to make that judgement is to compare SPI to differences in SPI. Ideally, I want to watch competitive soccer with good teams.

```
league.sum <- pairs%>%
  group_by(league)%>%
  summarise(med.spi = median(spi.a),
            med.dif = median(spi_diff))
ggplot(league.sum, aes(med.dif, med.spi, label = league))+
  geom_text(size = 3)+
  theme_yaz()+
  labs(x = 'Median Pairwise SPI-Difference',
       y = 'Median SPI Rating',
       title = 'Competitiveness vs. Quality by League',
       caption = 'projects.fivethirtyeight.com/global-club-soccer-rankings/')
```

<p align="center">
  <img src="https://joshyazman.github.io/images/spi-ratings-2017/image4.png#center"/>
</p>

The teams in the top-left quadrant of the Competitiveness by League plot have the highest SPI ratings and the lowest differences between them. Spain’s La Liga, and Germany’s Bundesliga are the most exciting leagues to watch by this criteria. That doesn’t mean there won’t be fun games to watch from the British Premier League or MLS, but the best balances of competition and fun come from Germany and Spain right now.
