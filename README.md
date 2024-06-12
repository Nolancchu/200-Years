<html lang="en">
<head>
</head>
<body>


<!-- Header -->


<!-- First Grid -->
<h1 class="w3-text-white w3-jumbo">200 Years: An Analysis of <span class="br"></span>  Team Mean Champion Age</h1>
<p class="w3-text-white small w3-xlarge">DSC 80 Final Project: Nolan Chu & Desmond Vu</p>
<h1>Introduction</h1>
<h5 class="w3-padding-16 w3-left-align">League of Legends, colloquially know as LoL or Leauge, is a popular multiplayer online battle arena video game developed and published by Riot Games. It was released in 2009 and has since become one of the most widely played and watched esports games in the world.</h5>
<h5 class="w3-padding-16 w3-left-align">Our dataset is on professional League of Legends games in 2022 and contains 12 rows per recorded game, 2 for the summary statistics of each team, and 1 for each player on each team (10 players in total). Each row contains key statistics about the team or the player and gives insight into the game state and overall results. Our goal is to explore the dataset to find meaningful information for anyone who plays and cares about the game.</h5>
<h5 class="w3-padding-16 w3-left-align">League of legends players frequently joke/complain about how recently released champions are unbalanced, so we want to devle into the validity of that claim with the following question: <b><em>Does the time since your champion's release date have any effect on one's likelihood to win a game?</em></b> Is it possible to predict which team will win a game by their team's mean champion age, and can I use our findings to make excuses for when I lose in solo queue?</h5>
<h1>The Dataset</h1>
<h5 class="w3-padding-16 w3-left-align">Our dataset from Oracle's Elixir contains gameplay statistics and records on each match. The dataset contains 148992 rows, and 131 total columns, the most important to our research being gameid, result, date, champion, and position.</h5>
<ul class="w3-padding-16 w3-left-align">
<li>
                    <h5><i>gameid:</i> This column contains a unique identifier for each match, allowing to identify unique games within the dataset.</h5>
                    <br>
                </li>
                <li>
                    <h5><i>result:</i> This column contains either an integer 1 or and integer 0, representing whether the team who the row belongs to won that particular game. A 1 represents a win, and a 0 represents a loss.</h5>
                    <br>
                </li>
                <li>
                    <h5><i>date:</i> This column contains a timestamp with the year, month, day, hour, minute and second the match started. For the purposes or our research, we shortened it to only year, month, and day.</h5>
                    <br>
                </li>
                <li>
                    <h5><i>champion:</i> This column contains strings with the name of the champion played. This column contains a string if the row is a player row, and null if the row is a team row.</h5>
                    <br>
                </li>
                <li>
                    <h5><i>position:</i> This column contains strings representing the position played by a player. It can be any of the following: "top", "jng", "mid", "bot", or "sup". This column contains one of the prior strings if the row is a player row, and null if the row is a team row.</h5>
                    <br>
                </li>
            </ul>
            <h5 class="w3-padding-16 w3-left-align">In addition to the columns listed prior, that were part of the original dataset, we added several new columns to help us with our research, which are described below:</h5>
            <ul class="w3-padding-16 w3-left-align">
                <li>
                    <h5><i>release date:</i> This column contain timestamps with year, month, and day corresponding to the date which the champion being played was released. This column was also updated for any major gameplay reworks riot has released for that champion, replacing their release date with the time of that rework. The release/rework dates used was scraped from the League of Legends wiki. This column contains a timestamp if the row is a player row, and null if the row is a team row.</h5>
                    <br>
                </li>
                <li>
                    <h5><i>champion age:</i> This column contains time deltas, representing the time between a champion release date and the time when the match in the row was played. This column contains a time delta if the row is a player row, and null if the row is a team row.</h5>
                    <br>
                </li>
              
            </ul>
        
            <h1>Data Cleaning and Exploratory Data Analysis</h1>
        

        </div>
    </div>
</div>


</body>
</html>
