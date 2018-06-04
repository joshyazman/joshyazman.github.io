---
layout: post
title: "Exploring Lebron James' Point Share"
---

Depending on who you ask, Lebron James is the greatest basketball player of all time or at least in the top three. He is constantly compared to [Micheal Jordan](https://trends.google.com/trends/explore?date=all&geo=US&q=Lebron%20James%20vs%20Michael%20Jordan) and has, at the time of publication, played in the last 9 NBA Finals and won both championships and MVP recognitions in in 2012, 2013, and 2016. LeBron James' was drafted first overall out of St. Vincent's Academy in the 2003-2004 NBA draft by the Cleveland Cavaliers. He had an immediate impact, dropping 27 points in his first game, then winning Rookie of the Year season and contributing to an 18-win improvement for the Cavs over the prior year. 

But several of those championships were won with NBA super-teams. He had all-star teammates like Dwayne Wade and Chris Bosh to help out in Miami and Kyrie Irving in Cleveland*. This past season Lebron James has been seen as playing on a team with a collection of gomers who don't really contribute much beyond his abilities. In fact, it was a major story when, after winning the Eastern Conference finals, Lebron credited his teammates with [helping](https://www.usatoday.com/story/sports/ftw/2018/05/27/lebron-james-passionately-defended-his-teammates-after-the-cavs-run-to-the-nba-finals/111168656/) [Cleveland](https://www.ohio.com/akron/sports/cavs/nba-finals-lebron-james-bristling-at-criticisms-of-cavaliers-teammates) [win](https://sports.theonion.com/lebron-james-credits-teammates-with-providing-4-bodies-1825658316)!

<p> 
  <img src="https://joshyazman.github.io/images/lebron-point-share/Lebron James Point Share Over Time.png#center"/>
</p>

James is undeniably one of the greatest single basketball players to step onto the hardwood, but how does his reputation for putting his team on his back play out in point production? Using data from the [NBA stats API and the `statsnbaR` package](https://github.com/stephematician/statsnbaR), we can calculate James' point totals as a percentage of his team's point totals for each of the 1380 games he's played. The answer is remarkably consistent across his career. James has averaged 27.5 points per game, accounting for about 27% of all points scored by his teams in Cleveland and Miami. He's won three NBA championships with two teams and was selected for 14 All-Star games. But ultimately it's not clear that additional hero-ball from James is helpful.

There is some fluctiation, but no real trends over time, in LeBron James' share of points scored per game (point share). His average contribution by season ranges from 22% of points scored per game in his 2004 rookie season to 32% of points scored in 2006. In 2018, James typically scored about 26% of points scored by the Cavaliers per game. One player making up over a quarter of a team's offensive production is still incredibly impressive, but James doesn't shoulder more of the scoring burden for the current Cavs roster than past teams.  

<p> 
  <img src="https://joshyazman.github.io/images/lebron-point-share/Lebron James Point Share Playoffs vs Regular.png#center"/>
</p>

But funny things start to happen in the playoffs. James has played in each of the last 12 NBA playoffs with two separate teams. In seasons James plays with the Cavaliers, his typical share of playoff point totals ranges from 25-36% while his share of playoff point totals during his time with the Miami Heat ranged from 25-31%.

With the exception of two years with Miami, James has consistently stepped up his share of point production.This season, the difference has been a remarkable 10% increase in James' point share the regular season to the post-season.

That said, the marginal impact of hero ball on point spreads (`points for - points against`) has not always been positive. In fact a 1% increase in point share has mostly been associated with a his team scoring fewer points relative to opponents including 2012, 2013, and 2016 when teams led by James won the NBA title. This metric has wide confidence intervals, so it's hard to conclude too much, but this metric does indicate that perhaps Lebron really does need a strong supporting cast around him to go all the way - despite his Herculean run in the 2018 NBA playoffs.

<p> 
  <img src="https://joshyazman.github.io/images/lebron-point-share/Marginal Impact.png#center"/>
</p>

James' point share has remained fairly steady throughout his career and his performance this year isn't much different from his average point share. Also, there isn't enough data to conclusively determine whether greater point share for James is a good thing. But James' point share indicates a few things fairly conclusively. He has always been an incredibly high impact scoring threat on every team he's layed for and he scores a higher percentage of points for the Cavaliers in the playoffs than he does in the regular season. 

* Granted, MJ had some help from guys like Steve Kerr, Scottie Pippen, and many other strong supporting players as well as a literal Zen Master as a coach!
