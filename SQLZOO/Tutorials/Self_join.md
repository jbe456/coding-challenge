## 1.
Summary
How many stops are in the database.

```sql
SELECT Count(1) 
FROM   stops 
```

## 2.
Find the id value for the stop 'Craiglockhart'

```sql
SELECT id 
FROM   stops 
WHERE  name = 'Craiglockhart' 
```

## 3.
Give the id and the name for the stops on the '4' 'LRT' service.

```sql
SELECT id, 
       name 
FROM   route 
       JOIN stops 
         ON stop = id 
WHERE  company = 'LRT' 
       AND num = 4 
ORDER  BY pos 
```

## 4. Routes and stops
The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these stops have a count of 2. Add a HAVING clause to restrict the output to these two routes.

```sql
SELECT company, 
       num, 
       Count(1) AS nb 
FROM   route 
WHERE  stop IN ( 149, 53 ) 
GROUP  BY company, 
          num 
HAVING nb = 2 
```

## 5.
Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.

```sql
SELECT a.company, 
       a.num, 
       a.stop, 
       b.stop 
FROM   route a 
       JOIN route b 
         ON ( a.company = b.company 
              AND a.num = b.num ) 
WHERE  a.stop = 53 
       AND b.stop = 149 
```

## 6.
The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'

```sql
SELECT a.company, 
       a.num, 
       b.name, 
       d.name 
FROM   route a 
       JOIN stops b 
         ON a.stop = b.id 
       JOIN route c 
         ON a.num = c.num 
            AND a.company = c.company 
       JOIN stops d 
         ON c.stop = d.id 
WHERE  b.name = 'Craiglockhart' 
       AND d.name = 'London Road' 
```

## 7. Using a self join
Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')

```sql
SELECT DISTINCT a.company, 
                a.num 
FROM   route a 
       JOIN route b 
         ON ( a.company = b.company 
              AND a.num = b.num ) 
WHERE  a.stop = 115 
       AND b.stop = 137 
```

## 8.
Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'

```sql
SELECT DISTINCT a.company, 
                a.num 
FROM   route a 
       JOIN stops b 
         ON a.stop = b.id 
       JOIN route c 
         ON a.num = c.num 
            AND a.company = c.company 
       JOIN stops d 
         ON c.stop = d.id 
WHERE  b.name = 'Craiglockhart' 
       AND d.name = 'Tollcross' 
```

## 9.
Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.

```sql
SELECT DISTINCT d.name, 
                a.company, 
                a.num 
FROM   route a 
       JOIN stops b 
         ON a.stop = b.id 
       JOIN route c 
         ON a.num = c.num 
            AND a.company = c.company 
       JOIN stops d 
         ON c.stop = d.id 
WHERE  b.name = 'Craiglockhart' 
       AND a.company = 'LRT' 
```

## 10.
Find the routes involving two buses that can go from Craiglockhart to Lochend.
Show the bus no. and company for the first bus, the name of the stop for the transfer,
and the bus no. and company for the second bus.

```sql
SELECT a.num, 
       a.company, 
       d.name, 
       f.num, 
       f.company 
FROM   route a 
       JOIN stops b 
         ON a.stop = b.id 
       JOIN route c 
         ON a.num = c.num 
            AND a.company = c.company 
       JOIN stops d 
         ON c.stop = d.id 
       JOIN route e 
         ON c.stop = e.stop 
       JOIN route f 
         ON e.num = f.num 
            AND e.company = f.company 
       JOIN stops g 
         ON f.stop = g.id 
WHERE  b.name = 'Craiglockhart' 
       AND g.name = 'Lochend' 
ORDER  BY a.num, 
          a.company, 
          d.name, 
          f.num, 
          f.company 
```

