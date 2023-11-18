# League-Of-Legends-Early-Stats-Analysis
This is a project for DSC80 where we analyze missing values, do permutation tests, and clean a League of Legends dataset. 

**Name(s)**:  Quennie Zeng and Vaibhav Bommisetty

---

## Introduction

We worked on a dataset of all professional competitive League of Legends matches that took place in 2022. In this dataset, every 12 rows are of the same game, of which 10 are for player statistics on each team and 2 are for team statistics. In total, this dataset contains 149,400 rows, alluding to 12,450 games in total.
Our question is: “Which early game statistic can help us determine the outcome of a game the most?”
To help us find the answer to our question, we used 4 different categories of statistics: 

### 1. Outcome of Game

We needed a column to let us know which team won the game, that column was “result”: 
- **`result`** - The value in this column lets us know if the team won or not. We cleaned the data so that if the team won the value is True, and if the team lost the value is False.

### 2. First to Objective Statistics:

These statistics give us insight into the team objectives in the early game. All of these columns contain booleans, as the objectives are either achieved or not achieved.

- **`firstblood`** - The value is True if the team achieved first blood, and False if the team did not achieve first blood. (A bonus of 150 gold is given to the player who gets first blood)
- **`firstdragon`** - The value is True if the team claimed the first dragon, and False if the team did not claim the first dragon. (The first dragon spawns at exactly 5 minutes into the match)
- **`firstherald`** - The value is True if the team claimed the first herald, and False if the team did not claim the first herald. (The first dragon spawns at exactly 8 minutes into the match)
- **`firsttower`**  - The value is True if the team destroyed their first tower and False if the team did not destroy the first tower. (This statistic is an early game statistic, even though it depends on how the game is going. 11 minutes is a good estimation of when the first tower is taken.)

### 3. Statistics at 10 Minutes:

These statistics are gathered at exactly 10 minutes into the game.

- **`goldat10`** - This gives the gold count of the team at 10 minutes.
- **`xpat10`** - This gives the xp of the team at 10 minutes.
- **`csat10`** - This gives the cs (creep score) of the team at 10 minutes.
- **`golddiffat10`** - This gives the difference in the gold between the teams at 10 minutes. If it is positive, the team we are looking at has more gold and vice versa.
- **`xpdiffat10`** - This gives the difference in the xp between the teams at 10 minutes. The positive and negative meanings are similar to the above column.
- **`csdiffat10`** - This gives the difference in the cs between the teams at 10 minutes. The positive and negative meanings are similar to the above column.
- **`killsat10`** - This gives the number of kills that the team has at 10 minutes.
- **`assistsat10`**  - This gives the number of assists that the team has at 10 minutes.

### 4. Statistics at 15 minutes:

Similar to the previous section, this section gives team statistics for 15 minutes.

- **`goldat15`** - This gives the gold count of the team at 15 minutes.
- **`xpat15`** - This gives the xp of the team at 15 minutes.
- **`csat15`** - This gives the cs (creep score) of the team at 15 minutes.
- **`golddiffat15`** - This gives the difference in the gold between the teams at 15 minutes. If it is positive, the team we are looking at has more gold and vice versa.
- **`xpdiffat15`** - This gives the difference in the xp between the teams at 15 minutes. The positive and negative meanings are similar to the above column.
- **`csdiffat15`** - This gives the difference in the cs between the teams at 15 minutes. The positive and negative meanings are similar to the above column.
- **`killsat15`** - This gives the number of kills that the team has at 15 minutes.
- **`assistsat15`** - This gives the number of assists that the team has at 15 minutes.

---

## Cleaning and EDA

### Data Cleaning:

We decided to keep the columns above. When we first started looking at the data, we noticed that the number of wins and the number of losses were not the same, there were four more losses than wins, meaning that for two games there was no winner.

This is the head of our df dataframe:

| gameid | datacompleteness | url | league | year | split | playoffs | date | game | patch | ... | opp_csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 | opp_killsat15 | opp_assistsat15 | opp_deathsat15 |
|--------|-------------------|-----|--------|------|-------|----------|------|------|-------|-----|------------|--------------|------------|------------|-----------|-------------|------------|---------------|------------------|-----------------|
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 121.0 | 391.0 | 345.0 | 14.0 | 0.0 | 1.0 | 0.0 | 0.0 | 1.0 | 0.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 100.0 | 541.0 | -275.0 | -11.0 | 2.0 | 3.0 | 2.0 | 0.0 | 5.0 | 1.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 119.0 | -475.0 | 153.0 | 1.0 | 0.0 | 3.0 | 0.0 | 3.0 | 3.0 | 2.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 149.0 | -793.0 | -1343.0 | -34.0 | 2.0 | 1.0 | 2.0 | 3.0 | 3.0 | 0.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 21.0 | 443.0 | -497.0 | 7.0 | 1.0 | 2.0 | 2.0 | 0.0 | 6.0 | 2.0 |
| ESPORTSTMNT01_2690210 | complete | NaN | LCKC | 2022 | Spring | 0 | 2022-01-10 07:44:08 | 1 | 12.01 | ... | 135.0 | -391.0 | -345.0 | -14.0 | 0.0 | 1.0 | 0.0 | 0.0 | 1.0 | 0.0 |





Games had either both winners or both losers:

| gameid | datacompleteness | url | league | year | split | playoffs | date | game | patch | ... | opp_csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 | opp_killsat15 | opp_assistsat15 | opp_deathsat15 |
|--------|-------------------|-----|--------|------|-------|----------|------|------|-------|-----|------------|--------------|------------|------------|-----------|-------------|------------|---------------|------------------|-----------------|
| 34918 | ESPORTSTMNT04_2170436 | complete | NaN | ESLOL | 2022 | Spring | 0 | 2022-03-02 17:22:03 | 1 | 12.04 | ... | 530.0 | 2824.0 | -795.0 | 16.0 | 2.0 | 4.0 | 4.0 | 4.0 | 5.0 | 2.0 |
| 34919 | ESPORTSTMNT04_2170436 | complete | NaN | ESLOL | 2022 | Spring | 0 | 2022-03-02 17:22:03 | 1 | 12.04 | ... | 546.0 | -2824.0 | 795.0 | -16.0 | 4.0 | 5.0 | 2.0 | 2.0 | 4.0 | 4.0 |
| 87406 | ESPORTSTMNT03_2788015 | complete | NaN | ESLOL | 2022 | Summer | 0 | 2022-06-27 17:25:00 | 1 | 12.11 | ... | 522.0 | -916.0 | 885.0 | 7.0 | 1.0 | 0.0 | 2.0 | 2.0 | 3.0 | 1.0 |
| 87407 | ESPORTSTMNT03_2788015 | complete | NaN | ESLOL | 2022 | Summer | 0 | 2022-06-27 17:25:00 | 1 | 12.11 | ... | 529.0 | 916.0 | -885.0 | -7.0 | 2.0 | 3.0 | 1.0 | 1.0 | 0.0 | 2.0 |

We then proceeded to find sources to see who was the winner in both of these games:
Sector One won their game against LG UltraGear<sub>1</sub> And KRC Genk Esports won their game against LowLandLions~We then proceeded to find sources to see who was the winner in both of these games:
Sector One won their game against LG UltraGear.<sub>1</sub> And KRC Genk Esports won their game against LowLandLions <sub>2</sub>

1. [Sector One v mCon LG UltraGear](https://www.youtube.com/watch?v=fm7QNRBEcRI)
2. [KRC Genk Esports v LowLandLions](https://www.strafe.com/match/krc-genk-esports-vs-lowlandlions-regular-regular-summer-2022-f7a887/)

After we fixed the inaccuracy in the data, we then turned all columns that contain 1s and 0s into booleans. These included the columns: `result` and all the first to objective statistics. 

*Note: To keep only relevant columns, we removed the data that included information on late-game statistics or opposing team statistics. Since we were only looking to see what statistics in the early game impacted the outcome of the game, we removed columns such as 'firstbaron', as that is a late-game objective. We also removed opposing team statistics, as that could be found in the other team’s row. The removed columns were focused on late-game objectives and opposing team statistics, which were not relevant to the analysis of early game outcomes.*

So after examining the data and data cleaning, this is what the head of dataframe we would be working with looks like.

In this dataframe, we went through each 10th and 11th row, as that would be the results of each team, not player. Then we took out the relevant columns. When cleaning our data, we found a discreptancy within the results, where there were two games where the result was not recorded. So, we went back to the recording of the games and watched the games to individually fill out the result column. We changed the 1 and 0 to boolean on columns such as `firstdragon` to make it easier to comprehend. 

| gameid              | datacompleteness        | league | playoffs | result | firstblood | firstdragon | firstherald | firsttower | goldat10 | ... | deathsat10 | goldat15 | xpat15 | csat15 | golddiffat15 | xpdiffat15 | csdiffat15 | killsat15 | assistsat15 | deathsat15 |
|---------------------|-------------------------|--------|----------|--------|------------|-------------|-------------|------------|----------|-----|------------|----------|--------|--------|--------------|------------|------------|-----------|-------------|------------|
| 10                  | ESPORTSTMNT01_2690210   | complete | LCKC  | False  | False      | True        | False       | True       | 16218.0  | ... | 0.0        | 24806.0  | 28001.0 | 487.0  | -1617.0      | -23.0      | 5.0        | 10.0      | 6.0         | 0.0        |
| 11                  | ESPORTSTMNT01_2690210   | complete | LCKC  | False  | True       | False       | True        | False      | 14695.0  | ... | 3.0        | 24699.0  | 29618.0 | 510.0  | -107.0       | 1617.0     | 23.0       | 6.0       | 18.0        | 5.0        |
| 22                  | ESPORTSTMNT01_2690219   | complete | LCKC  | False  | False      | False       | False       | True       | 14939.0  | ... | 3.0        | 23522.0  | 28848.0 | 533.0  | -1763.0      | -906.0     | -22.0      | 1.0       | 1.0         | 3.0        |
| 23                  | ESPORTSTMNT01_2690219   | complete | LCKC  | False  | True       | True        | True        | False      | 16558.0  | ... | 1.0        | 25285.0  | 29754.0 | 555.0  | 1763.0       | 906.0      | 22.0       | 3.0       | 3.0         | 1.0        |
| 34                  | 8401-8401_game_1        | partial | LPL   | False  | True       | False       | NaN         | NaN        | NaN      | ... | NaN        | NaN      | NaN    | NaN    | NaN          | NaN        | NaN        | NaN       | NaN         | NaN        |




### EDA
#### Univariate Testing:
<iframe src="assets/Percentage_of_Wins_with_First_Tower_pie_chart.html" width="800" height="600"></iframe>
<iframe src="assets/Percentage_of_Wins_with_First_Herald_pie_chart.html" width="800" height="600"></iframe>
<iframe src="assets/Percentage_of_Wins_with_First_Dragon_pie_chart.html" width="800" height="600"></iframe>
<iframe src="assets/Percentage_of_Wins_with_First_Blood_pie_chart.html" width="800" height="600"></iframe>

These pie charts show the proportion of games won when a team secures an early game objective.

### Bivariate Testing



<iframe src="assets/Gold_Diff_at_15_Winning_v_Losing_Histogram.html" width="800" height="600"></iframe>

From these histograms, we can see that winning teams have a higher, more likely positive, gold difference than losing teams.

<iframe src="assets/early_game_object_freq_of_winning_teams_bar_chart.html" width="800" height="600"></iframe>


These bar graphs show the proportion of winning teams that secure early game objectives. Different from the pie charts from before, these bar charts focus on the proportion of winning teams that claim each specific objective, as the pie charts focus on the win rate of teams that get certain early-game objectives. As you can see, more winning teams get First Blood and First Tower than First Dragon or First Herald.

### Interesting Aggregates

This `groupby()` highlights the mean number of early statistics when teams lose or win. The first row is when the team loses and the second row is when the team wins. We can see the distribution between winning and losing with a particular early stat. For instance, we are much more likely to win with `firsttower` than win without it. This aggregate lets us see if we got an early stat and what are the chances of winning. 

| result | first_dragon_mean | first_blood_mean | first_herald_mean | first_tower_mean | goldat10 | xpat10 | csat10 | killsat10 | assistsat10 | deathsat10 | goldat15 | xpat15 | csat15 | killsat15 | assistsat15 | deathsat15 |
|--------|-------------------:|------------------:|-------------------:|------------------:|---------:|-------:|-------:|----------:|------------:|------------:|---------:|-------:|-------:|----------:|------------:|------------:|
| False  |            0.423102 |           0.390553 |            0.417458 |           0.316997 | 15343.21 | 17986.81 | 310.04 |    1.77067 |     2.69307 |     2.60380 | 23963.84 | 28893.52 | 493.83 |    3.22754 |      5.20929 |      4.93162 |
| True   |            0.576710 |           0.607920 |            0.582259 |           0.683003 | 16033.79 | 18412.27 | 318.96 |    2.59562 |     3.98128 |     1.77735 | 25688.54 | 29926.60 | 511.14 |    4.92061 |      7.96557 |      3.23657 |


---

## Assessment of Missingness

### NMAR Analysis

Yes, we believe that many columns in our data are Not Missing At Random due to the trends we see in the data. After cleaning the data and seeing how some pieces of data are missing due to the column itself, we can see that statistics are missing for certain losing teams and not missing for certain winning teams. This is also similar the other way around, meaning that the missingness of the rows depend on the type of columns and many other columns as well. This correlation cannot be based on another column's missingness (for the columns we are talking about), so it would not be missing by design. For other columns like `killsat15`, we see that it is MAR, where it is dependent on other columns, where one is `league` as leagues like LPL are missing this stat constantly. 

We also know some information are MAR like `firstdragon` and `golddiffat15` as we can see a correlation with these and `league`. For certain leagues, all the info for these columns are missing, so we can determine that `leagues` play a part in the missingness of other columns. 

### Missingness Dependency:

#### Missingness of ‘firstdragon’ depends on ‘league’:

Missingness of `firstdragon` does depend on `league`. We wanted to determine if `league` and `league` were Missing at Random or Missing Completely at Random.

Here is the observed distribution when `firstdragon` was not missing:

| league    | firstdragon |
|-----------|-------------|
| CBLOL      | 0.022858    |
| CBLOLA    | 0.020318    |
| CDF       | 0.006867    |
| CT        | 0.002446    |
| DDH       | 0.019659    |
| EBL       | 0.017402    |
| EL        | 0.012699    |
| ESLOL     | 0.022764    |
| EUM       | 0.025115    |
| GL        | 0.016367    |
| GLL       | 0.019095    |
| HC        | 0.015238    |
| HM        | 0.014392    |
| IC        | 0.007055    |
| LAS       | 0.021353    |
| LCK       | 0.043928    |
| LCKC      | 0.037061    |
| LCL       | 0.001505    |
| LCO       | 0.019942    |
| LCS       | 0.028784    |
| LCSA      | 0.050795    |
| LEC       | 0.022858    |
| LFL       | 0.023234    |
| LFL2      | 0.022670    |
| LHE       | 0.022858    |
| LJL       | 0.020130    |
| LJLA      | 0.003574    |
| LLA       | 0.017590    |
| LMF       | 0.030007    |
| LPLOL     | 0.019659    |
| LVP SL    | 0.023046    |
| MSI       | 0.007525    |
| NEXO      | 0.018154    |
| NLC       | 0.036027    |
| PCS       | 0.025491    |
| PGC       | 0.052864    |
| PGN       | 0.014016    |
| PRM       | 0.034522    |
| SL (LATAM)| 0.015521    |
| TAL       | 0.019283    |
| TCL       | 0.020788    |
| UL        | 0.026150    |
| UPL       | 0.038755    |
| VCS       | 0.030383    |
| VL        | 0.015991    |
| WLDs      | 0.013263    |

Here is the observed distribution when `firstdragon` was missing:

| league | firstdragon |
|--------|-------------|
| DCup   | 0.0         |
| LDL    | 0.0         |
| LPL    | 0.0         |
| WLDs   | 0.0         |



In this bar chart we can see how the missing values for the column ‘firstdragon’ is concentrated in four leagues, with the majority being in two leagues: LPL and LDL.


<iframe src="assets/Missingness_of_firstdragon_barchart.html" width="800" height="600"></iframe>


Our observed statistic was: 0.9923034634414514

Our p-value was: 0.0

If the p-value is significant (below the chosen significance level), we may reject the null hypothesis. This suggests that the missingness is not completely random, and there may be a systematic pattern or relationship with observed or unobserved variables. This situation is more indicative of missing at random (MAR) or missing not at random (MNAR).

Here is the empirical distribution of the test statistic:

<iframe src="assets/emprical_distribution_of_the_tvd_for_firstdragon_and_league%20(2).html" width="800" height="600"></iframe>



#### Missingness of `firstdragon` does not depend on `firstblood`

We wanted to determine if `firstdragon` and `firstblood` were Missing at Random or Missing Completely at Random.

Here is the observed distribution when `firstdragon` and `firstblood` was missing and not missing:

|              | firstdragon_missing = False | firstdragon_missing = True  |
|--------------|-----------------------------|------------------------------|
| **firstblood** |                             |                              |
| False        | 0.500705                    | 0.5011                       |
| True         | 0.499295                    | 0.4989                       |

When we graph this, we see that missingness is close to each other when a team loses or wins. 

<iframe src="assets/firstblood_v_missingness_of_first_dragon_barchart.html" width="800" height="600"></iframe>

After running our tests, we get 

Our observed statistic was: 0.00039462604900317166
Our p-value was: 0.948

Since the p-value is above the chosen significance level, p-value from the test is not significant and we fail to reject the null hypothesis. In this case, we might conclude that the data is missing completely at random (MCAR). This makes sense as in our bar graph, we can see that the missingness does not look like they have any relation with each other. 

Here is the empirical distribution of the test statistic:

<iframe src="assets/empirical_distribution_of_the_tvd_firstblood_vs_missingness_of_firstdragon%20(1).html" width="800" height="600"></iframe>

---

## Hypothesis Testing
**Null Hypothesis**: The distribution of early game statistics where the team won is the same as the distribution of early game statistics where the team lost.

**Alternate Hypothesis**: The distribution of early game statistics where the team won is difference than the distribution of early game statistics where the team lost.

**Test Statistic**: We will be using the Total Variation Distance (TVD).

We decided to use Total Variation Distance because we are using categoricial data, more specifically whether our sample distributions came from the same distribution. Our significance level is going to be 0.05 as it is the standard significance level. 


We found the distribution of early game statistics and how the amount of points can correlate with the result of the game. 

| points | 0.0      | 1.0      | 2.0      | 3.0      | 4.0      | 5.0      | 6.0      | 7.0      | 8.0      | 9.0      | 10.0     |
| ------ | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| result |          |          |          |          |          |          |          |          |          |          |          |
| False  | 0.909385 | 0.84707  | 0.759547 | 0.687318 | 0.589577 | 0.495258 | 0.408747 | 0.311444 | 0.241554 | 0.147168 | 0.09106  |
| True   | 0.090615 | 0.15293  | 0.240453 | 0.312682 | 0.410423 | 0.504742 | 0.591253 | 0.688556 | 0.758446 | 0.852832 | 0.90894  |

Looking at this through a bar chart,

<iframe src="assets/distribution_bar_plot_of_points_for_win_and_loss.html" width="800" height="600"></iframe>

Our observed statistic was: 0.05610845677532025
Our p-value: 0.0

Here is the empirical distribution of the test statistic: 
<iframe src="assets/empirical_distribution_of_hypothesis_test.html" width="800" height="600"></iframe>

For our hypothesis testing, we did 500 simulations and were able to see that the p-value is significant as it is smaller than 0.05. 

Conclusion: We reject the null hypothesis, meaning we believe that the distributions are not the same.

Since we may reject the null hypothesis this means there is a good amount of evidence that the distribution of early game statistics in games where the team won is different from the distribution of statistics where the team won. 

