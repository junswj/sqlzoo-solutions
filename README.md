# sqlzoo.net - solutions
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


---
# <a name ="sumcount"> SUM and COUNT </a>: 


---
# <a name ="join"> JOIN </a>:

---
# <a name = "morejoin"> More JOIN operations </a>: 

---
# <a name = "usingnull"> Using Null </a>: 

---
# <a name = "selfjoin"> Self join </a>:
