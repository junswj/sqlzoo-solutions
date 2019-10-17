# sqlzoo solutions
Answers to the questions from https://sqlzoo.net/

[**SELECT basics:**](#basics)<br>
Some simple queries to get you started

[**SELECT from World:**](#world)<br>
In which we query the World country profile table.

[**SELECT from Nobel:**](#nobel)<br>
Additional practice of the basic features using a table of Nobel Prize winners.

[**SELECT in SELECT:**](#select)<br>
In which we form queries using other queries.

[**SUM and COUNT:**](#sumcount)<br>
In which we apply aggregate functions. more the same

[**JOIN:**](#join)<br>
In which we join two tables; game and goals. previously music tutorial

[**More JOIN operations:**](#morejoin)<br>
In which we join actors to movies in the Movie Database.

[**Using Null:**](#usingnull)<br>
In which we look at teachers in departments. previously Scottish Parliament

[**Self join:**](#selfjoin)<br>
In which we join Edinburgh bus routes to Edinburgh bus routes.

---
# <a name="basics"> SELECT basics </a>:
1. Modify it to show the population of Germany
```sql
SELECT population 
FROM world 
WHERE name = 'Germany';
```

2. Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.
```sql
SELECT name, population 
FROM world 
WHERE name IN ('Sweden', 'Norway', 'Denmark');
```

3. Show the country and the area for countries with an area between 200,000 and 250,000.
```sql
SELECT name, area 
FROM world
WHERE area BETWEEN 200000 AND 250000;
```

---
# <a name ="world"> SELECT from World </a>:
1. show the name, continent and population of all countries.
```sql
SELECT name, continent, population 
FROM world;
```

2. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
```sql
SELECT name FROM world
WHERE population >= 200000000;
```

3. Give the name and the per capita GDP for those countries with a population of at least 200 million.
```sql
SELECT name, gdp/population 
FROM world 
WHERE population >= 200000000;
```

4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
```sql
SELECT name, population/1000000 
FROM world 
WHERE continent = 'South America';
```

5. Show the name and population for France, Germany, Italy
```sql
SELECT name, population 
FROM world 
WHERE name in ('France', 'Germany', 'Italy');
```

6. Show the countries which have a name that includes the word 'United'
```sql
SELECT name 
FROM world 
WHERE name LIKE '%United%';
```

7. Show the countries that are big by area or big by population. Show name, population and area.
```sql
SELECT name, population, area
FROM world 
WHERE area >=3000000 OR population >=250000000;
```

8. Exclusive OR (XOR). Show the countries that are big by area or big by population but not both. Show name, population and area.
```sql
SELECT name, population, area 
FROM world 
WHERE area >= 3000000 XOR population >250000000;
```

9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'.
```sql
SELECT name, 
ROUND(population/1000000,2), ROUND(gdp/1000000000,2)
FROM world 
WHERE continent = 'South America';
```

10. Show per-capita GDP for the trillion dollar countries to the nearest $1000.
```sql
SELECT name, ROUND(gdp/population, -3) 
FROM world 
WHERE gdp >= 1000000000000;
```

11. Show the name and capital where the name and the capital have the same number of characters.
```sql
SELECT name, capital 
FROM world 
WHERE LENGTH(name) = LENGTH(capital);
```

12. Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
```sql
SELECT name, capital 
FROM world 
WHERE LEFT(name, 1) = LEFT(capital,1) AND name <> capital;
```

13. Find the country that has all the vowels and no spaces in its name.
```sql
SELECT name
   FROM world
WHERE 
    name LIKE '%e%'
    AND name LIKE '%a%'
    AND name LIKE '%u%'
    AND name LIKE '%i%'
    AND name LIKE '%o%'
    AND name NOT LIKE '% %'
```
---
# <a name ="nobel"> SELECT from Nobel </a>: 
1. Change the query shown so that it displays Nobel prizes for 1950.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950;
 ```

 2. Show who won the 1962 prize for Literature.
 ```sql
SELECT winner
FROM nobel
WHERE yr = 1962 AND subject = 'Literature'
```

3. Show the year and subject that won 'Albert Einstein' his prize.
```sql
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein';
```

4. Give the name of the 'Peace' winners since the year 2000, including 2000.
```sql
SELECT winner
FROM nobel
WHERE subject = 'Peace' AND yr >= 2000;
```

5. Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'Literature' AND yr BETWEEN 1980 and 1989;
```

6. Show all details of the presidential winners:
> Theodore Roosevelt, Woodrow Wilson, Jimmy Carter, Barack Obama
```sql
SELECT * FROM nobel
WHERE winner IN ('Theodore Roosevelt',
                'Woodrow Wilson',
                'Jimmy Carter',
                'Barack Obama');
```

7. Show the winners with first name John
```sql
SELECT winner
FROM nobel
WHERE winner LIKE 'John%';
```

8. Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984.
```sql
SELECT yr, subject, winner
FROM nobel
WHERE 
(subject = 'Physics' AND yr = 1980)
OR
(subject ='Chemistry' AND yr = 1984);
```

9. Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine
```sql
SELECT yr, subject, winner
FROM nobel
WHERE subject <>  'Chemistry' 
AND subject <> 'Medicine'
AND yr = 1980;
```

10. Show year, subject, and name of people who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
```sql
SELECT yr, subject, winner
FROM nobel
WHERE (subject = 'Medicine' AND yr <1910)
OR (subject = 'Literature' AND yr >=2004);
```

11. Find all details of the prize won by PETER GRÜNBERG
```sql
SELECT * FROM nobel WHERE winner = 'PETER GRÜNBERG';
```

12. Find all details of the prize won by EUGENE O'NEILL
```sql
SELECT * FROM nobel WHERE winner = 'EUGENE O''NEILL';
```

13. List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```sql
SELECT winner, yr, subject 
FROM nobel
WHERE winner LIKE 'Sir%'
ORDER BY 2 DESC, 1; 
```

14. Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.
```sql
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Chemistry' , 'Physics'), subject, winner;
```

---
# <a name ="select"> SELECT in SELECT </a>: 
1. List each country name where the population is larger than that of 'Russia'.
```sql
SELECT name FROM world 
WHERE population > 
(SELECT population FROM world WHERE name = 'Russia');
```

2. Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
SELECT name FROM world
WHERE (gdp/population > 
      (SELECT gdp/population 
       FROM world WHERE name = 'United Kingdom')) 
       AND continent = 'Europe';
```

3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
SELECT name, continent FROM world 
WHERE continent IN 
    (SELECT continent FROM world 
    WHERE name IN ('Argentina', 'Australia')) 
ORDER BY name;
```

4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.
```sql
SELECT name, population FROM world 
WHERE  
    population > 
    (SELECT population FROM world WHERE name = 'Canada')
    AND
    population < 
    (SELECT population FROM world WHERE name = 'Poland');
```

5. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```sql
SELECT name, 
CONCAT(
    ROUND(
        population/
        (SELECT population FROM world 
        WHERE name = 'Germany')*100, 0), '%') 
FROM world WHERE continent = 'EUROPE'
```

6. Which countries have a GDP greater than every country in Europe? 
```sql
SELECT name FROM world 
WHERE gdp > 
    (SELECT MAX(gdp) FROM world 
    WHERE continent = 'Europe');
```

7. Find the largest country (by area) in each continent, show the continent, the name and the area:
```sql
SELECT continent, name, area FROM world 
WHERE area = ANY
(SELECT MAX(area) FROM world GROUP BY continent);
```

8. List each continent and the name of the country that comes first alphabetically.
```sql
SELECT continent, name FROM world 
WHERE name = ANY (SELECT MIN(name) 
                    FROM world GROUP BY continent);
```

9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
SELECT name, continent, population FROM world ow
WHERE 25000000 >= ALL (SELECT population FROM world iw
                         WHERE ow.continent = iw.continent AND population IS NOT NULL);
```

10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
```sql
SELECT name, continent FROM world x 
WHERE x.population >=  ALL (SELECT population*3 FROM world y 
                            WHERE x.continent = y.continent 
                            AND x.name <> y.name);
```

---
# <a name ="sumcount"> SUM and COUNT </a>: 
1. Show the total population of the world.
```sql
SELECT SUM(population) FROM world;
```

2. List all the continents - just once each.
```sql
SELECT DISTINCT(continent) FROM world;
```

3. Give the total GDP of Africa
```sql
SELECT SUM(gdp) FROM world WHERE continent = 'Africa';
```

4. How many countries have an area of at least 1000000
```sql
SELECT COUNT(name) FROM world WHERE area >=1000000;
```

5. What is the total population of ('Estonia', 'Latvia', 'Lithuania')
```sql
SELECT SUM(population) FROM world 
WHERE name IN ('Estonia', 'Latvia', 'Lithuania');
```

6. For each continent show the continent and number of countries.
```sql
SELECT continent, COUNT(name) FROM world GROUP BY continent;
```

7. For each continent show the continent and number of countries with populations of at least 10 million.
```sql
SELECT continent, COUNT(name) FROM world 
WHERE population >= 10000000 GROUP BY continent;
```

8. List the continents that have a total population of at least 100 million.
```sql
SELECT continent FROM world 
GROUP BY continent 
HAVING sum(population) >= 100000000;
```

---
# <a name ="join"> JOIN </a>:
1. Show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
```sql
SELECT matchid, player FROM goal WHERE teamid = 'GER';
```

2. Show id, stadium, team1, team2 for just game 1012
```sql
SELECT id, stadium, team1, team2 
FROM game 
WHERE id =1012;
```

3. Show the player, teamid, stadium and mdate for every German goal.
```sql
SELECT go.player, go.teamid, ga.stadium, ga.mdate 
FROM goal go 
JOIN game ga ON go.matchid=ga.id 
WHERE go.teamid = 'GER';
```

4. Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
```sql
SELECT ga.team1, ga.team2, go.player 
FROM game ga 
JOIN goal go ON ga.id=go.matchid 
WHERE go.player LIKE 'Mario%';
```

5. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
```sql
SELECT go.player, go.teamid, et.coach, go.gtime 
FROM goal  go 
JOIN eteam et ON go.teamid = et.id 
WHERE go.gtime<=10;
```

6. List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
```sql
SELECT ga.mdate, et.teamname FROM game ga 
JOIN eteam et ON ga.team1 = et.id 
WHERE et.coach = 'Fernando Santos';
```

7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
```sql
SELECT go.player FROM goal go 
JOIN game ga ON go.matchid = ga.id 
WHERE ga.stadium = 'National Stadium, Warsaw';
```

8. Instead show the name of all players who scored a goal against Germany.
```sql
SELECT DISTINCT(player) FROM goal 
WHERE matchid IN (SELECT id FROM game 
                  WHERE team1 = 'GER' OR team2 = 'GER')
      AND teamid <> 'GER';
```

9. Show teamname and the total number of goals scored.
```sql
SELECT et.teamname, COUNT(go.player) FROM goal go 
JOIN eteam et ON go.teamid = et.id
GROUP BY 1;
```

10. Show the stadium and the number of goals scored in each stadium.
```sql
SELECT ga.stadium, count(go.player) FROM game ga 
JOIN goal go ON go.matchid = ga.id
GROUP BY 1;
```

11. For every match involving 'POL', show the matchid, date and the number of goals scored.
```sql
SELECT go.matchid, ga.mdate, COUNT(go.player) FROM goal go
JOIN game ga ON go.matchid = ga.id
WHERE ga.team1='POL' OR ga.team2='POL'
GROUP BY 1,2;
```

12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
```sql
SELECT go.matchid, ga.mdate, COUNT(go.player) FROM game ga
JOIN goal go ON ga.id = go.matchid
WHERE go.teamid = 'GER'
GROUP BY 1,2;
```

13. List every match with the goals scored by each team as shown.
```sql
SELECT ga.mdate,
ga.team1, 
SUM(CASE WHEN go.teamid=ga.team1 THEN 1 ELSE 0 END) SCORE1, 
ga.team2,
SUM(CASE WHEN go.teamid=ga.team2 THEN 1 ELSE 0 END) SCORE2
FROM game ga
LEFT JOIN goal go
ON ga.id = go.matchid
GROUP BY 1,go.matchid,2,4
ORDER BY 1,go.matchid,2,4;
```
---
# <a name = "morejoin"> More JOIN operations </a>: 
1. List the films where the yr is 1962 [Show id, title]
```sql
SELECT id, title 
FROM movie
WHERE yr=1962;
```

2. Give year of 'Citizen Kane'.
```sql
SELECT yr 
FROM movie 
WHERE title = 'Citizen Kane';
```

3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
```sql
SELECT id, title, yr 
FROM movie 
WHERE title LIKE '%Star Trek%' ORDER BY 3;
```

4. What id number does the actor 'Glenn Close' have?
```sql
SELECT id FROM actor WHERE name = 'Glenn Close';
```

5. What is the id of the film 'Casablanca'
```sql
SELECT id FROM movie WHERE title ='Casablanca';
```

6. Obtain the cast list for 'Casablanca'.
```sql
SELECT ac.name FROM actor ac 
WHERE ac.id IN (SELECT ca.actorid 
                FROM casting ca WHERE movieid=11768);
```

7. Obtain the cast list for the film 'Alien'
```sql
SELECT ac.name FROM actor ac 
JOIN casting  ca ON ca.actorid = ac.id 
JOIN movie mo ON mo.id = ca.movieid
WHERE mo.title = 'Alien';
```

8. List the films in which 'Harrison Ford' has appeared
```sql
SELECT mo.title FROM movie mo
JOIN casting ca ON ca.movieid = mo.id
JOIN actor ac ON ac.id = ca.actorid
WHERE ac.name = 'Harrison Ford';
```

9. List the films where 'Harrison Ford' has appeared - but not in the starring role. 
```sql
SELECT mo.title FROM movie mo 
JOIN casting ca ON ca.movieid = mo.id
JOIN actor ac ON ac.id = ca.actorid
WHERE ac.name = 'Harrison Ford' AND ca.ord <> 1;
```

10. List the films together with the leading star for all 1962 films.
```sql
SELECT mo.title, ac.name FROM movie mo 
JOIN casting ca ON ca.movieid = mo.id
JOIN actor ac ON ac.id = ca.actorid
WHERE mo.yr = 1962 AND ca.ord = 1;
```

11. Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

```sql
SELECT mo.yr, COUNT(mo.title) FROM movie mo 
JOIN casting ca ON mo.id = ca.movieid
JOIN actor ac ON ac.id = ca.actorid
WHERE ac.name = 'Rock Hudson'
GROUP BY 1
HAVING COUNT(mo.title)>2;
```

12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.
```sql
SELECT mo.title, ac.name FROM movie mo
JOIN casting ca ON mo.id = ca.movieid
JOIN actor ac ON ac.id = ca.actorid
WHERE ca.movieid IN (SELECT ca.movieid FROM casting ca 
                     JOIN actor ac 
                     ON ac.id = ca.actorid 
                     WHERE ac.name = 'Julie Andrews')
                     AND ca.ord = 1;
```

13. Obtain a list, in alphabetical order, of actors who've had at least 30 starring roles.
```sql
SELECT ac.name FROM actor ac
WHERE ac.id IN (SELECT ca.actorid FROM casting ca
WHERE ca.ord = 1
GROUP BY 1
HAVING COUNT(ca.actorid) >=30)
ORDER BY 1;
```

14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.
```sql
SELECT mo.title, COUNT(ca.actorid) FROM movie mo 
JOIN casting ca ON ca.movieid = mo.id
WHERE yr = 1978
GROUP BY 1
ORDER BY 2 DESC, 1;
```

15. List all the people who have worked with 'Art Garfunkel'.
```sql
SELECT ac.name FROM actor ac 
JOIN casting ca ON ca.actorid = ac.id
WHERE ca.movieid IN 
(SELECT ca.movieid FROM casting ca 
JOIN actor ac ON ca.actorid = ac.id
WHERE ac.name = 'ART Garfunkel')
AND ac.name <> 'Art Garfunkel';
```

---
# <a name = "usingnull"> Using Null </a>: 
1. List the teachers who have NULL for their department.
```sql
SELECT te.name FROM teacher te WHERE te.dept IS NULL;
```

2. Note the INNER JOIN misses the teachers with no department and the departments with no teacher.
```sql
SELECT teacher.name, dept.name
FROM teacher 
INNER JOIN dept
ON (teacher.dept=dept.id)
```

3. Use a different JOIN so that all teachers are listed.
```sql
SELECT te.name, de.name FROM teacher te 
LEFT JOIN dept de ON de.id = te.dept;
```

4. Use a different JOIN so that all departments are listed.
```sql
SELECT te.name, de.name FROM teacher te 
RIGHT JOIN dept de ON de.id = te.dept;
```

5. Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given.
```sql
SELECT te.name, COALESCE(te.mobile, '07986 444 2266')
FROM teacher te;
```

6. Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where there is no department.
```sql
SELECT te.name, COALESCE(de.name, 'None') FROM teacher te
LEFT JOIN dept de ON te.dept = de.id;
```

7. Use COUNT to show the number of teachers and the number of mobile phones.
```sql
SELECT COUNT(name), COUNT(mobile) FROM teacher
```

8. Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.
```sql
SELECT de.name, COALESCE(COUNT(te.name), 0) FROM teacher te
RIGHT JOIN dept de ON de.id = te.dept
GROUP BY 1
```

9. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2 and 'Art' otherwise.
```sql
SELECT te.name, CASE WHEN dept IN (1, 2) THEN 'Sci' ELSE 'Art' END FROM teacher te;
```

10. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise.
```sql
SELECT te.name, 
CASE WHEN dept IN (1,2) THEN 'Sci'
WHEN dept = 3 THEN 'Art'
ELSE 'None' END
FROM teacher te;
```

---
# <a name = "selfjoin"> Self join </a>:
1. How many stops are in the database.
```sql
SELECT COUNT(id) FROM stops
```

2. Find the id value for the stop 'Craiglockhart'
```sql
SELECT id FROM stops WHERE name = 'Craiglockhart'
```

3. Give the id and the name for the stops on the '4' 'LRT' service.
```sql
SELECT id, name FROM stops WHERE id IN
(SELECT ro.stop FROM route ro WHERE num = '4' AND company = 'LRT');
```

4. The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these stops have a count of 2. Add a HAVING clause to restrict the output to these two routes.
```sql
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
HAVING COUNT(*) = 2
```

5. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.
```sql
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
WHERE a.stop=53 AND b.stop = (SELECT id FROM stops WHERE name = 'London Road');
```

6. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross'
```sql
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a 
JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' AND stopb.name = 'London Road';
```

7. Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')
```sql
SELECT DISTINCT(x.company), x.num FROM route x 
JOIN route y ON x.company=y.company AND x.num=y.num
WHERE x.stop = 115 AND y.stop = 137;
```

8. Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'
```sql
SELECT x.company, x.num FROM route x 
JOIN route y ON x.company = y.company AND x.num = y.num
WHERE x.stop = (SELECT id FROM stops WHERE name ='Craiglockhart')
AND y.stop= (SELECT id FROM stops WHERE name = 'Tollcross')
```

9. Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.
```sql
SELECT st.name, x.company, y.num FROM route x
JOIN route y ON x.company = y.company AND x.num=y.num
JOIN stops st ON y.stop = st. id
WHERE x.stop = (SELECT id FROM stops WHERE name = 'Craiglockhart')
```

10. Find the routes involving two buses that can go from Craiglockhart to Lochend.
Show the bus no. and company for the first bus, the name of the stop for the transfer,
and the bus no. and company for the second bus.
```sql
SELECT ro1.num, ro1.company, st.name, ro3.num, ro3.company FROM route ro1
JOIN route ro2 ON ro1.num = ro2.num AND ro1.company = ro2.company
JOIN (route ro3 
      JOIN route ro4 
      ON ro3.num =ro4.num AND ro3.company = ro4.company)
JOIN stops st ON st.id = ro2.stop
WHERE ro1.stop = (SELECT st1.id FROM stops st1 WHERE name = 'Craiglockhart')
AND ro4.stop = (SELECT st2.id FROM stops st2 WHERE name = 'Lochend')
AND ro2.stop=ro3.stop;
```