# Baseball Markov Chain Modeling

## Methods

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

We look at each of the starting 9 players' cumulative stats from a season: home runs, triples, doubles, singles, walks, outs. We then weight these stats against the players' number of plate appearances to determine the players' odds of achieving each of the possible offensive outcomes.

## Examples

1. **Sunny/Rainy Days Scenario**: Imagine that the average number of sunny days and rainy days over 200 days are both 100. If over a span of 200 days, we measured 108 sunny days and 92 rainy days, find the number of rainy and sunny days over the next 200-day span.

2. **NBA Shot Distribution Scenario**: In the NBA, more three-point shots are being taken at a rate of 3%, and thus, two-point shots are decreasing at a rate of 3%. If the percentage of two-point shots being taken in the league today is 76%, and the percentage of three-point shots is 24%. If, across the league, players take 200,000 shots, find the percentage of two and three-point shots that are taken in the NBA in 15 years.

## Applications

Markov chain modeling for baseball has been around for a long time, with the first reliable model being made by Allen R. Freeze in 1973. His model was groundbreaking because it simplified the game, allowing him to take a quantity-over-quality approach by simulating 200,000 games.

### Model Simplifications

To simplify the model, the researchers decided to ignore several factors that would impact the stats of an actual team over a season:
- Injuries
- Days off
- Ejections
- Trades
- Suspensions
- Pinch hitters
- Defensive substitutions

## Activity: Batting Order Simulation

### Player Stats

| Player | Position | At Bats | Outs | Walks | Singles | Doubles | Triples | Home Runs |
|--------|----------|---------|------|-------|---------|---------|---------|-----------|
| Gus | Pitcher | 60 | 7 | 3 | 8 | 4 | 1 | 37 |
| Richie | Catcher | 60 | 37 | 15 | 8 | 0 | 0 | 0 |
| Clark | Field | 60 | 41 | 9 | 7 | 2 | 0 | 0 |

### Batting Order Simulation Results

| Lineup | Average Runs in Tournament |
|--------|----------------------------|
| Gus, Richie, Clark | 36.5 |
| Gus, Clark, Richie | 35.6 |
| Richie, Gus, Clark | 34.5 |
| Richie, Clark, Gus | 34.5 |
| Clark, Gus, Richie | 34.1 |
| Clark, Richie, Gus | 33.7 |

**Best Lineup**: Gus, Richie, Clark (averaging 36.5 points per game)

## Conclusion

The researchers used a Markov chain to simulate different batting orders for nine players. They found the most successful lineup to be:
1. Tauchman
2. Hoerner
3. Happ
4. Suzuki
5. Swanson
6. Morel
7. Bellinger
8. Gomes
9. Madrigal

This lineup scored approximately 6.65 runs per game, which is 1.6 more runs per game than the 2023 Cubs (who scored 5.06 runs per game).

## References

1. Bukiet, Bruce, et al. "A Markov Chain Approach to Baseball." *Operations Research*, vol. 45, no. 1, Dec. 1994, pp. 14–23.
2. D'Angelo, John P. "Baseball and Markov Chains: Power Hitting and Power Series." *Notices of the AM* 57.4 (2010): 490-495.
3. Freeze, R. Allan. "An analysis of baseball batting order by Monte Carlo Simulation." *Operations Research*, vol. 22, no. 4, 25 June 1973, pp. 728–735.
4. "The Mathematically Optimal Batting Order." *Overtime Heroics*, 28 Apr. 2023.
