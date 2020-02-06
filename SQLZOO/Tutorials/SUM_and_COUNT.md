## 1. Total world population
Show the total population of the world.

```sql
SELECT Sum(population) 
FROM   world 
```

## 2. List of continents
List all the continents - just once each.

```sql
SELECT continent 
FROM   world 
GROUP  BY continent 
```

## 3. GDP of Africa
Give the total GDP of Africa

```sql
SELECT Sum(gdp) 
FROM   world 
WHERE  continent = "africa" 
```

## 4. Count the big countries
How many countries have an area of at least 1000000

```sql
SELECT Count(name) 
FROM   world 
WHERE  area > 1000000 
```

## 5. Baltic states population
What is the total population of ('Estonia', 'Latvia', 'Lithuania')

```sql
SELECT Sum(population) 
FROM   world 
WHERE  name IN ( 'Estonia', 'Latvia', 'Lithuania' ) 
```

## 6. Counting the countries of each continent
For each continent show the continent and number of countries.

```sql
SELECT continent, 
       Count(name) 
FROM   world 
GROUP  BY continent 
```

## 7. Counting big countries in each continent
For each continent show the continent and number of countries with populations of at least 10 million.

```sql
SELECT continent, 
       Count(name) 
FROM   world 
WHERE  population > 10000000 
GROUP  BY continent 
```

## 8. Counting big continents
List the continents that have a total population of at least 100 million.

```sql
SELECT continent 
FROM   world 
GROUP  BY continent 
HAVING Sum(population) > 100000000 
```
