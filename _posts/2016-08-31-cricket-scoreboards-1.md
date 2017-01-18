---
layout: post
title: Generating cricket scoreboards I
image:
  feature: sample-image-44.jpg
  credit: Corfe Castle, England, 2016
---

I've always loved the sport of cricket, specifically the 5-day test match format, and been in awe of its ability to generate statistics. Experienced commentators and writers on the game are usually able to find **some** record being broken in every single game being played, whether it is based on a well-known statistic (highest individual or team score), or a more obscure one (most runs conceded by a bowler in the fourth innings for no wickets). 

I started to wonder about automatically "simulating" cricket matches. I quickly understood that doing this properly is an extremely demanding exercise in statistics and modelling that people have done PhDs in. This is for two main reasons:

- there are an astounding amount of variables at any given moment in a cricket match (pitch, ground, home team, crowd, day, weather, fatigue, bowler skill, bowler handedness, ball speed, ball condition, batter skill, batter handedness, etc etc...)
- there are several points in a cricket match where teams may have to make a strategic decision to declare or to enforce the follow-on and these decisions are very difficult to model as they depend mostly on captains' "intuition" or "feel" for the state of the match (and can include information about future weather or player injuries)

With a full simulator out of the question I decided the first step was to develop a basic simulation engine in Python which could produce logically accurate test match scorecards. This was a fun Python exercise which involved the following steps:

1. Encode the logic of a cricket match result (including declarations, the follow-on, the time constraints and all possible results including draws)
2. Build a class structure which includes a match class, a team class, a player class, a delivery class, and a class which keeps track of the match score.
3. Build a basic loop which allows the player to make basic decisions at any point, or to randomly simulate through a given number of deliveries. 

The next step (here) is to generate a large dataset of matches and see how hard it is to find records for individual matches. 

**The match class**

```
class Match(object):

    def __init__(self, ground, match_score, teams):
        self._day = 0
        self._session = 0
        self._session_over = 0
        self._innings = 0
        self._innings_over = 0
        self._ground = ground
        self._match_score = match_score
        self._teams = teams
```

The match class is initiated with a number of variables which keep track of the game's status: the most important of these is the scoreboard which is stored as an instance of a separate MatchScore class.

**The scoreboard class**

Most of the simulator's logic is contained in the scoreboard class.

```
class MatchScore(object):

    def __init__(self):
        self._scorecard = [[0,0]]
        self._declared = [False]
        self._is_follow_on = [False]
        self._is_over = False
```

The most important method of this class checks at the end of each delivery whether the match is over.

```
    def check_result(self, match):

        self._innings_number = len(self._scorecard)

        # If the time has run out, the match is drawn.

        if match._day == 5 and match._session == 3 and match._session_over == 30:
            return 'Match drawn.'

        # If, in the third innings, the team batting first has lost all their wickets and not surpassed the team batting second's total, the team batting second wins by an innings. 

        elif self._innings_number == 3 and self._scorecard[2][0] == 0 and self._scorecard[2][1] == 0 and (self._scorecard[0][1] + self._scorecard[2][1]) < self._scorecard[1][1]:
            return 'Team B won by an innings and ' + str(self._scorecard[1][1] - self._scorecard[2][1] - self._scorecard[0][1]) + ' runs.'

        elif self._innings_number >= 4:

            first_team_total = self._scorecard[0][1] + self._scorecard[2][1]
            second_team_total = self._scorecard[1][1] + self._scorecard[3][1]

            if self._scorecard[3][0] == 10:

                # If, in the fourth innings, all wickets are taken, then the team with the highest total wins or the match is tied. 

                if first_team_total > second_team_total:
                    self._is_over = True
                    return 'Team A won by ' + str(first_team_total - second_team_total) + ' runs.'

                elif first_team_total == second_team_total: 
                    self._is_over = True
                    return 'Match tied.'

            else:

                # If, in the fourth innings, the team batting second surpasses the team batting first's total, the team batting second wins. 

                if second_team_total > first_team_total:
                    self._is_over = True
                    return 'Team B won by ' + str(10 - self._scorecard[3][0]) + ' wickets.'
```

**The player class**

A player 






