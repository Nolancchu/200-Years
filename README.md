<!-- Header -->


<!-- First Grid -->
<h1>Introduction</h1>

<p>League of Legends, colloquially know as LoL or Leauge, is a popular multiplayer online battle arena video game developed and published by Riot Games. It was released in 2009 and has since become one of the most widely played and watched esports games in the world.</p>

<p>Our dataset is on professional League of Legends games in 2022 and contains 12 rows per recorded game, 2 for the summary statistics of each team, and 1 for each player on each team (10 players in total). Each row contains key statistics about the team or the player and gives insight into the game state and overall results. Our goal is to explore the dataset to find meaningful information for anyone who plays and cares about the game.</p>

<p>League of legends players frequently joke/complain about how recently released champions are unbalanced, so we want to devle into the validity of that claim with the following question: <em>Does the time since your champion's release date have any effect on one's likelihood to win a game?</em> Is it possible to predict which team will win a game by their team's mean champion age, and can I use our findings to make excuses for when I lose in solo queue?</p>

<h3>The Dataset</h3>

<p>Our dataset from Oracle's Elixir contains gameplay statistics and records on each match. The dataset contains 148992 rows, and 131 total columns, the most important to our research being gameid, result, date, champion, and position.</p>

<ul>
  <li>
    <p><em>gameid:</em> This column contains a unique identifier for each match, allowing to identify unique games within the dataset.</p>
  </li>
  <li>
    <p><em>result:</em> This column contains either an integer 1 or and integer 0, representing whether the team who the row belongs to won that particular game. A 1 represents a win, and a 0 represents a loss.</p>
  </li>
  <li>
    <p><em>date:</em> This column contains a timestamp with the year, month, day, hour, minute and second the match started. For the purposes or our research, we shortened it to only year, month, and day.</p>
  </li>
  <li>
    <p><em>champion:</em> In League of Legends, each player selects a character to play, called a champion. This column contains strings with the name of the champion played. This column contains a string if the row is a player row, and null if the row is a team row.</p>
  </li>
  <li>
    <p><em>position:</em> This column contains strings representing the position played by a player. It can be any of the following: "top", "jng", "mid", "bot", or "sup". This column contains one of the prior strings if the row is a player row, and null if the row is a team row.</p>
  </li>
</ul>

<p>In addition to the columns listed prior, that were part of the original dataset, we added several new columns to help us with our research, which are described below:</p>

<ul>
  <li>
    <p><em>release date:</em> This column contain timestamps with year, month, and day corresponding to the date which the champion being played was released. This column was also updated for any major gameplay reworks riot has released for that champion, replacing their release date with the time of that rework. The release/rework dates used was scraped from the League of Legends wiki. This column contains a timestamp if the row is a player row, and null if the row is a team row.</p>
  </li>
  <li>
    <p><em>champion age:</em> This column contains time deltas, representing the time between a champion release date and the time when the match in the row was played. This column contains a time delta if the row is a player row, and null if the row is a team row.</p>
  </li>
</ul>
      
<h1>Data Cleaning and Exploratory Data Analysis</h1>

<h3>Cleaning</h3>

<p>For our purposes, we needed to add to our dataset the date that each champion was released to the public. Additionally, we needed to update these dates for reworks or large gameplay updates released by Riot. Periodically, Riot makes substantial changes to certain champions, changing their gameplay and viability. Therefore, in order to reflect this in our dataset, we accounted for reworks/updates when scraping from the League of Legends wiki when creating our 'release date' column. </p>

<p>To create our 'champion age' column we created a pandas time delta between the date of the match and the date of the champion's release or latest major update.</p>

<p>Additionally, not all columns in the original dataset are needed, so we filtered the dataset to only contain our prior columns and the following: 'gameid', 'result', 'date', 'release date', 'champion age','champion', and 'position'. Shown below are the first few rows of our cleaned dataset.</p>

| gameid                | result | date       | release date | champion age | champion | position |
|-----------------------|--------|------------|--------------|--------------|----------|----------|
| ESPORTSTMNT01_2690210 | 0      | 2022-01-10 | 2011-01-18   | 4010 days    | Renekton | top      |
| ESPORTSTMNT01_2690210 | 0      | 2022-01-10 | 2017-09-26   | 1567 days    | Xin Zhao | jng      |
| ESPORTSTMNT01_2690210 | 0      | 2022-01-10 | 2016-11-10   | 1887 days    | LeBlanc  | mid      |
| ESPORTSTMNT01_2690210 | 0      | 2022-01-10 | 2020-09-21   | 476 days     | Samira   | bot      |
| ESPORTSTMNT01_2690210 | 0      | 2022-01-10 | 2011-07-13   | 3834 days    | Leona    | sup      |


<h3>Univariate Analysis</h3>
<p>We performed univariate analysis on the kills column, the graph below shows the frequency of each amount of kills</p>

<iframe
  src="killsfig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<h3>Bivariate Analysis</h3>
<p>We performed bivariate analysis on the champion and result column, except we transformed the results into winrate percentages for each champion.</p>

<iframe
  src="wrfig"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<h3>Aggregates</h3>
<p>Below we've aggregated the mean number of minion kills (called creep score or cs) per position in a game.</p>

| position |  total cs  |
|----------|------------|
| bot      | 278.649565 |
| jng      | 173.146585 |
| mid      | 260.918371 |
| sup      | 35.421472  |
| top      | 247.535760 |

<h1>Assessment of Missingness</h1>

<h3>NMAR Analysis</h3>
<p>We believe there to be a column in our original dataset that is NMAR, that being the url column. Some entries in the url column are missing, likely due to there not being a recording of that particular game. To further explain this point, we would need to prove whether or not a recording exists of each game.</p>

<h3>Missingness Dependency</h3>
<p>Another question we wanted to explore surronding missingness is the missingness of the 'baron' column. In game, the baron is an objectiive that a team can take that does not spawn until 20 minutes, so we wanted to run a permutation test with 'barons' and 'deaths'. Can you predict, at least to some degree, whether the barons entry will be missing or not looking at the amount of deaths recorded.</p>

<p>The answer is yes, the mean deaths when barons is null is 2.8 while the mean deaths when barons is non-null is 5.2, giving us a p-value of 0 for our permutation test. Below we've graphed out the average deaths for null and non-null barons entries.</p>

<iframe
  src="deathfig"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<h1>Hypothesis Testing</h1>
<ul>
  <li>Null hypothesis: time since a champion's release has NO bearing on their team's winrate</li>
  <li>Alternate hypothesis: time since a champion's release affects likelihood that their team will win.</li>
  <li>Test statistic: abs(winning team mean champion age - losing team mean champion age)</li>
  <li>Confidence Level: 95%</li>
</ul>

<p>For this test, our resulting p-test was 0.00, meaning that champion age definitely has SOME effect on winrate, though from this test alone, we can determine any more.</p>

<h1>Framing a Prediction Problem</h1>
<h4>Our problem: predicting the outcome of a match using champion age and role.</h4>
<p>To answer our problem, we're going to use a binary classifier on results, with the two classes being 1 representing a win, and 0 representing a loss. We chose this column because it's the most intutitve to work with and fits well with the theme and question of this project.</p>
<h4>Our metric: accuracy</h4>
<p>Because there are roughly the same amount of wins and losses in the results column, we're going to use accuracy in order to judge the quality of our models.</p>

<h1>Baseline Model</h1>

<p>For our base model, we wanted to keep it simple and see if we could predict the result of a match based on a team's champion ages, position, and kills, in total, 3 features. We changed champion ages from time deltas into ints, we one hot encoded position, and we left kills as is. All in all, there are two quantative features and one ordinal feature. Our model utilized a tree classifier.</p>

<p>For both our training and test sets, our base model achieved an accuracy of roughly 68%, which while not necessarily bad, and certainly higher than if guessing by chance, can be improved greatly, which we do in our following model.</p>

<h1>Final Model</h1>

<p>To our final model, we added the following features:</p>
<ul>
  <li>kills: kills are a good way to gauge someone's performance in a game.</li>
  <li>deaths: deaths, or a lack of, are a good way to gauge someone's performance in a game.</li>
  <li>assists: like kills and deaths, assists are a good way to gauge someone's performance in a game.</li>
  <li>gamelength: this feature was added to standardize kills, death, and assists as a longer game time allows players more time to fight in a match. A by group standardizer was used to standardize kills, deaths and assists based on game time.</li>
  <li>champion age: though it was in the previous model, in the final model it is now standardized.</li>
  <li>side: In the League of Legends map, the two sides that team's play on are not exactly the same. We wanted to account for this in our final model, so we one hot enocded side as a feature.</li>
</ul>

<p>Our algorithim for our final model is a random forest classifier, with the following optimal hyperparameters determined by doing a grid search on the model:</p>
<ul>
  <li>n_estimators = 100</li>
  <li>max_depth = 13</li>
  <li>criterion = entropy</li>
</ul>

<p>Our accuracy for our final model is vastly better than our base model, having a training set accuracy of roughly 91% and a test set accuracy of roughly 88%. Overall, our final model is leaps and bounds ahead of our base model.</p>

<h1>Fairness Analysis</h1>
<p>To analyze the fairness of our model, we created two groups, rows with a champion age above 365 days, and rows with champion ages below or including 365 days. The evaluation metric was accuracy and the test statistic we decided on was the accuracy of the model for only older champions - the accuracy of the model for only newer champions. Our significance level was 95% and we got a p-value of 1, meaning we fail to reject the null hypothesis that says there is no difference between the models accuracy when it comes to champions older than a year and those younger.</p>
