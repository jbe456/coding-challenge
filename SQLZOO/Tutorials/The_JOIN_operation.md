## 1.
The first example shows the goal scored by a player with the last name 'Bender'. The * says to list all the columns in the table - a shorter way of saying matchid, teamid, player, gtime

Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'

```sql
SELECT matchid, 
       player 
FROM   goal 
WHERE  teamid = 'GER' 
```

## 2.
From the previous query you can see that Lars Bender's scored a goal in game 1012. Now we want to know what teams were playing in that match.

Notice in the that the column matchid in the goal table corresponds to the id column in the game table. We can look up information about game 1012 by finding that row in the game table.

Show id, stadium, team1, team2 for just game 1012

```sql
SELECT id, 
       stadium, 
       team1, 
       team2 
FROM   game 
WHERE  id = 1012 
```

## 3.
The code below shows the player (from the goal) and stadium name (from the game table) for every goal scored.

Modify it to show the player, teamid, stadium and mdate for every German goal.

```sql
SELECT player, 
       teamid, 
       stadium, 
       mdate 
FROM   game 
       JOIN goal 
         ON ( id = matchid ) 
WHERE  teamid = 'GER' 
```

## 4.
Use the same JOIN as in the previous question.

Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'

```sql
SELECT team1, 
       team2, 
       player 
FROM   game 
       JOIN goal 
         ON ( id = matchid ) 
WHERE  player LIKE 'Mario %' 
```

## 5.
The table eteam gives details of every national team including the coach. You can JOIN goal to eteam using the phrase goal JOIN eteam on teamid=id

Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10

```sql
SELECT player, 
       teamid, 
       coach, 
       gtime 
FROM   goal 
       JOIN eteam 
         ON teamid = id 
WHERE  gtime <= 10 
```

## 6.
List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.

```sql
SELECT mdate, 
       teamname 
FROM   game 
       JOIN eteam 
         ON team1 = eteam.id 
WHERE  coach = 'Fernando Santos' 
```

## 7.
List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'

```sql
SELECT player 
FROM   goal 
       JOIN game 
         ON matchid = id 
WHERE  stadium = 'National Stadium, Warsaw' 
```

## 8.
The example query shows all goals scored in the Germany-Greece quarterfinal.
Instead show the name of all players who scored a goal against Germany.

```sql
SELECT DISTINCT player 
FROM   goal 
       JOIN game 
         ON matchid = id 
WHERE  teamid != 'GER' 
       AND ( team1 = 'GER' 
              OR team2 = 'GER' ) 
```

## 9.
Show teamname and the total number of goals scored.

```sql
SELECT teamname, 
       Count(matchid) 
FROM   eteam 
       JOIN goal 
         ON id = teamid 
GROUP  BY teamname 
```

## 10.
Show the stadium and the number of goals scored in each stadium.

```sql
SELECT stadium, 
       Count(matchid) 
FROM   game 
       JOIN goal 
         ON id = matchid 
GROUP  BY stadium 
```

## 11.
For every match involving 'POL', show the matchid, date and the number of goals scored.

```sql
SELECT matchid, 
       mdate, 
       Count(matchid) 
FROM   game 
       JOIN goal 
         ON matchid = id 
WHERE  ( team1 = 'POL' 
          OR team2 = 'POL' ) 
GROUP  BY matchid 
```

## 12.
For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'

```sql
SELECT matchid, 
       mdate, 
       Count(matchid) 
FROM   game 
       JOIN goal 
         ON matchid = id 
WHERE  teamid = 'GER' 
GROUP  BY matchid 
```

## 13.
List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.

```sql
SELECT mdate, 
       team1, 
       Sum(CASE 
             WHEN teamid = team1 THEN 1 
             ELSE 0 
           end) AS score1, 
       team2, 
       Sum(CASE 
             WHEN teamid = team2 THEN 1 
             ELSE 0 
           end) AS score2 
FROM   game 
       JOIN goal 
         ON matchid = id 
GROUP  BY id 
ORDER  BY mdate, 
          matchid, 
          team1, 
          team2 
```

