# sqlzoo.net - solutions
Answers to the questions from https://sqlzoo.net/

**0 SELECT basics:**<br>
Some simple queries to get you started

**1 SELECT name:**<br>
Some pattern matching queries

**2 SELECT from World:**<br>
In which we query the World country profile table.

**3 SELECT from Nobel:**<br>
Additional practice of the basic features using a table of Nobel Prize winners.

**4 SELECT within SELECT:**<br>
In which we form queries using other queries.

**5 SUM and COUNT:**<br>
In which we apply aggregate functions. more the same

**6 JOIN:**<br>
In which we join two tables; game and goals. previously music tutorial

**7 More JOIN operations:**<br>
In which we join actors to movies in the Movie Database.

**8 Using Null:**<br>
In which we look at teachers in departments. previously Scottish Parliament

**9 Self join:**<br>
In which we join Edinburgh bus routes to Edinburgh bus routes.

---
# 0 SELECT basics:
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