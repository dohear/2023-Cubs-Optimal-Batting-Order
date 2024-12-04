# 2023 Cubs Optimal Batting Order Simulation Using Markov Chain Modeling

## Overview
in this repository cubs-optimal-battingorder.rmd uses a Markov Chain model to simulate baseball games and calculate expected runs for a specified batting order based on player statistics. This can be easily altered to use any teams players, and be used to simulate any number of batting orders. This was done for my into to modeling class at Marquette University in collaberation with fellow student Joel Raupp, below the requirements I have put a full write up explaining our methodologies, goals and ultimately findings. The R code was all written by me, Daniel O'Hear, and the report below was developed in collaberation Joel Raupp, whos baseball knowledge and insight helped guide this project.  

## Requirements
- R programming language
- Libraries: `data.table`, `Matrix`


## Baseball Markov Chain Modeling
##### By Joel Raupp & Daniel O'Hear

### Executive Summary

We looked at the possibility of creating a more effective batting order for a team, in our 
case, the 2023 Chicago Cubs, and see if it could produce more runs than the actual team did in 
the season. We decided to take nine players from the team, one from each position, based on the 
number of games they played. Whoever played in the most games at each position would be 
designated to that position and be featured in the lineup.  
The stats we used are all base stats in baseball: at-bats, outs, walks, singles, doubles, 
triples, and home runs. Using these stats and by using a transition matrix to keep a log of who is 
on base allows us to accurately calculate the number of runs scored over a 162-game season. 
We used Markov chain to cycle players into every possible lineup scenario and calculated 
the number of runs scored by each lineup. We took that amount and compared it to the amount 
scored by the actual 2023 Chicago Cubs. Our lineup that we were able to generate scored about 
1.6 more runs per game than the actual 2023 Chicago Cubs. However, our model does not factor 
in reasons a player might not play in a game (i.e., injuries, suspensions, pinch hitters, etc.).

### Abstract

We used a Markov chain model to simulate batting orders using the 2023 Cubs roster to 
find out which batting order would score the most runs over a 162-game MLB season. We 
swapped players through the 1, 2, 3, 8, and 9 spot of the lineup because those spots in the order 
have the biggest effect on the runs scored by a team. We used base stats of the 2023 Chicago 
Cubs to perform this simulation. Our lineup scored approximately 6.65 runs per game, which 
was 1.6 runs more than the actual 2023 run total of the Chicago Cubs, 5.05. Our simulation, 
however, did not account for lineup substitutions, such as for injury or scheduled days off, so our 
simulation focuses on the nine players who played the most games at each position and finished 
the season with the Cubs. Thus, our run total from our simulation is likely inflated. 

### Introduction

Our problem was one that many sports fans find themselves exclaiming: “What on Earth 
is the coach doing?!” Both of us are big baseball fans, and Joel, in particular, is a big fan of the 
Chicago Cubs. This 2023 season, the Cubs narrowly missed the playoffs, and Joel believes it can 
be chalked up to some questionable lineups that Cubs manager, David Ross, trotted out onto the 
field. We wanted to look at if we could, through Markov chain code, create a lineup that was 
more productive than the Cubs were this season. 
We looked at the Cubs roster this season and selected the nine players who started the 
most games at each position. These nine players will be the only ones used for this test. We will 
test all possible batting orders with these 9 players, and measure each one’s productivity. While 
not a set formula, the typical batting order chemistry consists of your best contact/speed hitters in 
the top two spots, your three best power hitters in spots 3, 4, and 5, decent hitters in the 7th and 
8th spots, and your weakest hitter at the 9th spot. This is the mindset David Ross appeared to stick 
to with his lineups. Our test will also determine if this formula is actually the best way to arrange 
your batting order, or if this long-lasting idea should be changed. 
We are using all base stats in baseball: at-bats, outs, walks, singles, doubles, triples, and 
home runs. Using these stats and by using a transition matrix to keep a log of who is on base 
allows us to accurately calculate the number of runs scored over a 162-game season. We would 
have liked to test all possible batting orders (9!), but with that needing us to run a complex 
simulation 362880 times we had to limit it due to computational limitations. We limited the 
different batting orders to only moving around the 2 best hitters, the worst hitter, and a grouping 
of the remaining players ordered randomly, as the position of the 2 best hitters and worst hitter 
have the greatest impact on the lineup's success.  

### Methods

Markov Chain modeling proves to be one of the best choices when modeling baseball due to both the fact Baseball has a finite number of states and the fact that the next state is fully determined based on the current state. The model aims to find the lineup that scores the most points over a season, and as it has been widely agreed upon by both fans and experts alike, scoring more points directly correlates to more wins.

The model looks at every possible state during the offensive half of an inning. There are:
- 3 different possible numbers of outs (0, 1, 2)
- 8 different possible formations of players on base

When taking into account these factors, there are 24 possible states in one offensive half inning.

```
((on_base_formations * outs) * number_of_innings) + game_ending_state = number_of_possible_states
```

Thus, there are 217 possible states over the course of a game:
```
((8 * 3) * 9) + 1 = 217
```

You then need to create the transition matrix to manipulate the model through its different states. To do this, you need to look at what all of the possible outcomes of any given at-bat can be. There are 6 different possibilities:
1. Home run
2. Triple
3. Double
4. Single
5. Walk
6. Out

We look at each of the starting 9 players' cumulative stats from a season: home runs, triples, doubles, singles, walks, outs. We then weight these stats against the players' number of plate appearances to determine the players' odds of achieving each of the possible offensive outcomes.  You can then use the players 
weighted offensive stats as a transition matrix to manipulate the games state. Through this you 
can then extrapolate the amount of points a team scores with another matrix that checks if any 
players cross home plate in between two states.  

### Applications

Markov chain modeling for baseball has been around for a long time, with the first reliable model being made by Allen R. Freeze in 1973. His model was groundbreaking because it simplified the game, allowing him to take a quantity-over-quality approach by simulating 200,000 games. Our model takes the simplification a step further, using players' stats over the course of a season to calculate their average performance 
and hence the average outcome of a game over that season. In order to simplify the model we 
also decided to ignore a lot of major factors that would impact the stats of an actual team over a 
season. We ignored injuries, Days off, Ejections, Trades, Suspensions, Pinch hitters, and 
Defensive substitutions. Given the simplifications and what factors are being left out in our 
model, there is room for many different variations on the model in order to increase the accuracy 
of the results. We optimized our model to perfectly fit the scope of what was possible in this 
project, with us simplifying the model as little as possible in order to maintain the simulations 
relative accuracy, while still allowing us to run at least 24 simulations to find a teams optimal 
batting order position for the players that create the most impact. While it is likely that most 
MLB teams are using similar models for their teams, they probably only use them as a reference 
or starting point as there are so many different situations and dynamics at play in a team that a 
computer cannot even begin to account for.  

### Activity: Batting Order Simulation

The Benchwarmers just finished playing in “Mel's Tournament of Little Baseballers and Three 
Older Guys”. They won all 10 games and the championship, but is it possible for them to have 
won in an even more dominant fashion? The three-man lineup consisted mainly of:

#### Player Stats

| Player | Position | At Bats | Outs | Walks | Singles | Doubles | Triples | Home Runs |
|--------|----------|---------|------|-------|---------|---------|---------|-----------|
| Gus | Pitcher | 60 | 7 | 3 | 8 | 4 | 1 | 37 |
| Richie | Catcher | 60 | 37 | 15 | 8 | 0 | 0 | 0 |
| Clark | Field | 60 | 41 | 9 | 7 | 2 | 0 | 0 |

#### Batting Order Simulation Results

| Lineup | Average Runs in Tournament |
|--------|----------------------------|
| Gus, Richie, Clark | 36.5 |
| Gus, Clark, Richie | 35.6 |
| Richie, Gus, Clark | 34.5 |
| Richie, Clark, Gus | 34.5 |
| Clark, Gus, Richie | 34.1 |
| Clark, Richie, Gus | 33.7 |

**Best Lineup**: Gus, Richie, Clark (averaging 36.5 points per game)

### Conclusion

We used the nine players who started the most games at each position and used a 
transition matrix that accounted for every possible outcome in an at-bat with every possible on
base scenario (i.e. runners on which bases and number of outs during at-bat). We used Markov 
chain to simulate many different batting orders, and we found the following one as the most 
successful: 
1. Tauchman
2. Hoerner
3. Happ
4. Suzuki
5. Swanson
7. Bellinger
8. Gomes
9. Madrigal

This lineup scored approximately 6.65 runs per game, which is 1.6 more runs per game than the 2023 Cubs (who scored 5.06 runs per game).

In our simulation, however, we were unable to account for some reasons a player would 
not be able to appear in the lineup, such as injuries, scheduled off days, suspensions, ejections, 
trades, or pinch hitters and defensive substitutions. This likely inflated our run total due to 
players with worse performance not being forced into the lineup due to another player’s absence. 
However, 1.6 more runs per game is quite a large margin. This would account for approximately 
259 more runs over the entire 162-game season than the actual total by the 2023 Chicago Cubs, 
so it is likely to assume that this lineup would still have scored more runs per game than the 2023 
Cubs.

### References

1. Bukiet, Bruce, et al. "A Markov Chain Approach to Baseball." *Operations Research*, vol. 45, no. 1, Dec. 1994, pp. 14–23.
2. D'Angelo, John P. "Baseball and Markov Chains: Power Hitting and Power Series." *Notices of the AM* 57.4 (2010): 490-495.
3. Freeze, R. Allan. "An analysis of baseball batting order by Monte Carlo Simulation." *Operations Research*, vol. 22, no. 4, 25 June 1973, pp. 728–735.
4. "The Mathematically Optimal Batting Order." *Overtime Heroics*, 28 Apr. 2023.
