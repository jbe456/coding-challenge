## 1. 1962 movies
List the films where the yr is 1962 [Show id, title]

```sql
SELECT id, 
       title 
FROM   movie 
WHERE  yr = 1962
```

## 2. When was Citizen Kane released?
Give year of 'Citizen Kane'.

```sql
SELECT yr 
FROM   movie 
WHERE  title = 'Citizen Kane' 
```

## 3. Star Trek movies
List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.

```sql
SELECT id, 
       title, 
       yr 
FROM   movie 
WHERE  title LIKE '%Star Trek%' 
ORDER  BY yr 
```

## 4. id for actor Glenn Close
What id number does the actor 'Glenn Close' have?


```sql
SELECT id 
FROM   actor 
WHERE  name = 'Glenn Close' 
```

## 5. id for Casablanca
What is the id of the film 'Casablanca'

```sql
SELECT id 
FROM   movie 
WHERE  title = 'Casablanca' 
```

## 6. Cast list for Casablanca
Obtain the cast list for 'Casablanca'.

```sql
SELECT name 
FROM   movie 
       JOIN casting 
         ON movie.id = movieid 
       JOIN actor 
         ON actorid = actor.id 
WHERE  movie.id = 11768 
```

## 7. Alien cast list
Obtain the cast list for the film 'Alien'

```sql
SELECT name 
FROM   movie 
       JOIN casting 
         ON movie.id = movieid 
       JOIN actor 
         ON actorid = actor.id 
WHERE  movie.title = 'Alien' 
```

## 8. Harrison Ford movies
List the films in which 'Harrison Ford' has appeared

```sql
SELECT title 
FROM   movie 
       JOIN casting 
         ON movie.id = movieid 
       JOIN actor 
         ON actorid = actor.id 
WHERE  actor.name = 'Harrison Ford' 
```

## 9. Harrison Ford as a supporting actor
List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]

```sql
SELECT title 
FROM   movie 
       JOIN casting 
         ON movie.id = movieid 
       JOIN actor 
         ON actorid = actor.id 
WHERE  actor.name = 'Harrison Ford' 
       AND ord != 1 
```

## 10. Lead actors in 1962 movies
List the films together with the leading star for all 1962 films.

```sql
SELECT title, 
       name 
FROM   movie 
       JOIN casting 
         ON movie.id = movieid 
       JOIN actor 
         ON actorid = actor.id 
WHERE  yr = 1962 
       AND ord = 1 
```

## 11. Busy years for Rock Hudson
Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

```sql
SELECT yr, 
       Count(title) 
FROM   movie 
       JOIN casting 
         ON movie.id = movieid 
       JOIN actor 
         ON actorid = actor.id 
WHERE  name = 'Rock Hudson' 
GROUP  BY yr 
HAVING Count(title) > 2 
```

## 12. Lead actor in Julie Andrews movies
List the film title and the leading actor for all of the films 'Julie Andrews' played in.

```sql
SELECT title, 
       name 
FROM   movie 
       JOIN casting 
         ON movieid = movie.id 
       JOIN actor 
         ON actorid = actor.id 
WHERE  ord = 1 
       AND movieid IN (SELECT movieid 
                       FROM   casting 
                              JOIN actor 
                                ON actorid = actor.id 
                       WHERE  name = 'Julie Andrews') 
```

## 13. Actors with 15 leading roles
Obtain a list, in alphabetical order, of actors who've had at least 15 starring roles.

```sql
SELECT name 
FROM   casting 
       JOIN actor 
         ON actorid = actor.id 
WHERE  ord = 1 
GROUP  BY actorid 
HAVING Count(movieid) >= 15 
ORDER  BY name 
```

## 14.
List the films released in the year 1978 ordered by the number of actors in the cast, then by title.

```sql
SELECT title, 
       Count(actorid) 
FROM   movie 
       JOIN casting 
         ON movieid = movie.id 
WHERE  yr = 1978 
GROUP  BY movieid 
ORDER  BY -Count(actorid), 
          title 
```

## 15.
List all the people who have worked with 'Art Garfunkel'.

```sql
SELECT DISTINCT name 
FROM   casting 
       JOIN actor 
         ON actorid = actor.id 
WHERE  name != 'Art Garfunkel' 
       AND movieid IN (SELECT movieid 
                       FROM   casting 
                              JOIN actor 
                                ON actorid = actor.id 
                       WHERE  name = 'Art Garfunkel') 
```
