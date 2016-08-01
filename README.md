# SELECT BASICS

1. The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes';

Modify it to show the population of Germany

SELECT population FROM world
  WHERE name = 'Germany'

2. Checking a list The word IN allows us to check if an item is in a list. The example shows the name and population for the countries 'Luxembourg', 'Mauritius' and 'Samoa'.

Show the name and the population for 'Ireland', 'Iceland' and 'Denmark'.

SELECT name, population FROM world
  WHERE name IN ('Ireland', 'Iceland', 'Denmark');

3. Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.

SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000

# SELECT from WORLD

1. Read the notes about this table. Observe the result of running a simple SQL command.

SELECT name, continent, population FROM world

2. How to use WHERE to filter records. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.

SELECT name FROM world
WHERE population>200000000

3. Give the name and the per capita GDP for those countries with a population of at least 200 million.

SELECT name, gdp/population
  FROM world
  WHERE population >= 200000000

4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.

SELECT name, population/1000000
  FROM world
  WHERE continent = "South America"

5. Show the name and population for France, Germany, Italy

SELECT name, population
  FROM world
  WHERE name IN ('France', 'Germany', 'Italy');

6. Show the countries which have a name that includes the word 'United'

SELECT name
  FROM world
  WHERE name LIKE '%United%'

7. Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.
Show the countries that are big by area or big by population. Show name, population and area.

SELECT name, population, area
  FROM world
  WHERE population > 250000000 OR area > 3000000

8. Exclusive OR (XOR). Show the countries that are big by area or big by population but not both. Show name, population and area.

Australia has a big area but a small population, it should be included.
Indonesia has a big population but a small area, it should be included.
China has a big population and big area, it should be excluded.
United Kingdom has a small population and a small area, it should be excluded.

SELECT name, population, area
  FROM world
  WHERE population > 250000000 XOR area > 3000000

9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places.

For South America show population in millions and GDP in billions both to 2 decimal places.
Millions and billions
Divide by 1000000 (6 zeros) for millions. Divide by 1000000000 (9 zeros) for billions.

SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2)
 FROM world
 WHERE continent = 'South America'

10. Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.

Show per-capita GDP for the trillion dollar countries to the nearest $1000.

SELECT name, ROUND(gdp/population, -3)
  FROM world
  WHERE gdp > 1000000000000

11. The CASE statement shown is used to substitute North America for Caribbean in the third column.

Show the name - but substitute Australasia for Oceania - for countries beginning with N.

SELECT name,
       CASE WHEN continent='Oceania' THEN 'Australasia'
            ELSE continent END
 FROM world
 WHERE name LIKE 'N%'

12. Show the name and the continent - but substitute Eurasia for Europe and Asia; substitute America - for each country in North America or South America or Caribbean. Show countries beginning with A or B

SELECT name,
              CASE WHEN continent IN ('Europe', 'Asia')
              THEN 'Eurasia'
              WHEN continent IN ('North America', 'South America', 'Caribbean')
              THEN 'America'
              ELSE continent END
  FROM world
  WHERE name LIKE 'A%' OR name LIKE 'B%'

13. Put the continents right...

Oceania becomes Australasia
Countries in Eurasia and Turkey go to Europe/Asia
Caribbean islands starting with 'B' go to North America, other Caribbean islands go to South America
Order by country name in ascending order
Show the name, the original continent and the new continent of all countries.

SELECT name, continent,
              CASE WHEN continent = 'Eurasia' OR name = 'Turkey' THEN 'Europe/Asia'
              WHEN continent = 'Caribbean' AND name LIKE 'B%' THEN  'North America'
              WHEN continent = 'Caribbean' AND name NOT LIKE 'B%' THEN  'South America'
              WHEN continent = 'Oceania' THEN 'Australasia'
              ELSE continent END AS "New Continent"
  FROM world
  ORDER BY name



# SELECT from NOBEL

1)  Change the query shown so that it displays Nobel prizes for 1950.


SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950


2) Show who won the 1962 prize for Literature.

SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'


3) Show the year and subject that won 'Albert Einstein' his prize.

SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein'


4) Give the name of the 'Peace' winners since the year 2000, including 2000.

SELECT winner
FROM nobel
Where yr >= 2000 AND subject = 'peace'

5) Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.

SELECT *
FROM nobel
WHERE subject = 'literature' AND yr BETWEEN 1980 AND 1989

6) Show all details of the presidential winners:

Theodore Roosevelt
Woodrow Wilson
Jimmy Carter

SELECT *
FROM nobel
 WHERE winner IN        ('Theodore Roosevelt',
                         'Woodrow Wilson',
                         'Jimmy Carter')

7) Show the winners with first name John
SELECT winner
FROM nobel
WHERE winner LIKE 'John %'

8) Show the Physics winners for 1980 together with the Chemistry winners for 1984.

SELECT *
FROM nobel
WHERE (subject = 'Physics' AND yr = 1980) OR (subject = 'Chemistry' and yr = 1984)

9) Show the winners for 1980 excluding the Chemistry and Medicine

SELECT *
FROM nobel
WHERE (subject != 'Chemistry' AND subject != 'Medicine') AND yr = 1980

10) Show who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)

SELECT *
FROM nobel
WHERE (subject = 'Medicine' AND yr < 1910) OR  (subject = 'Literature' AND yr >= 2004)

11) Find all details of the prize won by PETER GRÜNBERG

SELECT *
FROM nobel
WHERE winner = 'Peter Grünberg'

12) Find all details of the prize won by EUGENE O'NEILL

SELECT *
FROM nobel
WHERE winner = 'Eugene O\'neill'

13.
Knights in order

List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.

SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'SIR%'
ORDER BY yr DESC, winner

14. The expression subject IN ('Chemistry','Physics') can be used as a value - it will be 0 or 1.

Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.

SELECT winner, subject
  FROM nobel
  WHERE yr=1984
  ORDER BY subject IN ('Physics', 'Chemistry'), subject,winner

# The JOIN operation

1.
The first example shows the goal scored by a player with the last name 'Bender'. The * says to list all the columns in the table - a shorter way of saying matchid, teamid, player, gtime

Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'

SELECT matchid, player FROM goal
  WHERE teamid = 'GER'

2. From the previous query you can see that Lars Bender's scored a goal in game 1012. Now we want to know what teams were playing in that match.

Notice in the that the column matchid in the goal table corresponds to the id column in the game table. We can look up information about game 1012 by finding that row in the game table.

Show id, stadium, team1, team2 for just game 1012

SELECT id,stadium,team1,team2
  FROM game
  WHERE id = 1012

  3. You can combine the two steps into a single query with a JOIN.

  SELECT *
    FROM game JOIN goal ON (id=matchid)
  The FROM clause says to merge data from the goal table with that from the game table. The ON says how to figure out which rows in game go with which rows in goal - the id from goal must match matchid from game. (If we wanted to be more clear/specific we could say
  ON (game.id=goal.matchid)

  The code below shows the player (from the goal) and stadium name (from the game table) for every goal scored.

  Modify it to show the player, teamid, stadium and mdate and for every German goal.

SELECT player,teamid, stadium, mdate
  FROM game JOIN goal ON (id=matchid)
  WHERE teamid = 'GER'

4) Use the same JOIN as in the previous question.

Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'

SELECT team1,team2, player
  FROM game JOIN goal ON (id=matchid)
  WHERE player LIKE 'Mario%'

5) The table eteam gives details of every national team including the coach. You can JOIN goal to eteam using the phrase goal JOIN eteam on teamid=id

Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10

SELECT player, teamid, coach, gtime
  FROM goal JOIN eteam ON(id=teamid)
 WHERE gtime<=10

 6) To JOIN game with eteam you could use either
game JOIN eteam ON (team1=eteam.id) or game JOIN eteam ON (team2=eteam.id)

Notice that because id is a column name in both game and eteam you must specify eteam.id instead of just id

List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.

SELECT mdate, teamname
FROM game JOIN eteam ON (team1=eteam.id)
WHERE coach = 'Fernando Santos'

7)  List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'

SELECT player
FROM goal JOIN game ON (id=matchid)
WHERE stadium = 'National Stadium, Warsaw'

8) The example query shows all goals scored in the Germany-Greece quarterfinal.
Instead show the name of all players who scored a goal against Germany.

SELECT DISTINCT player
  FROM game JOIN goal ON matchid = id
    WHERE (team1='GER' OR team2='GER') AND teamid!='GER'

9)  Show teamname and the total number of goals scored.

SELECT teamname, COUNT(*)
  FROM eteam JOIN goal ON id=teamid
 GROUP BY teamname

10)  Show the stadium and the number of goals scored in each stadium.

 SELECT stadium, COUNT(*)
FROM goal JOIN game ON id=matchid
GROUP BY stadium

11) For every match involving 'POL', show the matchid, date and the number of goals scored.

SELECT matchid ,mdate, COUNT(*)
  FROM game JOIN goal ON (matchid = id)
  WHERE team1 = 'POL' OR team2 = 'POL'
  GROUP BY mdate, matchid

12) For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'

SELECT matchid ,mdate, COUNT(*)
  FROM game JOIN goal ON (matchid = id)
  WHERE (team1 = 'GER' OR team2 = 'GER') AND teamid = 'GER'
  GROUP BY mdate, matchid

13) 13.
List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.
mdate	team1	score1	team2	score2
1 July 2012	ESP	4	ITA	0
10 June 2012	ESP	1	ITA	1
10 June 2012	IRL	1	CRO	3
...
Notice in the query given every goal is listed. If it was a team1 goal then a 1 appears in score1, otherwise there is a 0. You could SUM this column to get a count of the goals scored by team1. Sort your result by mdate, matchid, team1 and team2.

SELECT mdate,
  team1,
  SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) AS team_1_goals,
  team2,
  SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) AS team_2_goals
  FROM game LEFT JOIN goal ON matchid = id
 GROUP BY mdate, matchid, team1, team2

 # More JOIN operations

1) List the films where the yr is 1962 [Show id, title]
SELECT id, title
 FROM movie
 WHERE yr=1962

 2)Give year of 'Citizen Kane'.
 SELECT yr
FROM movie
WHERE title = 'Citizen Kane'

3) List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.

SELECT id, title, yr
FROM movie
WHERE title LIKE 'STAR TREK%'
ORDER BY yr

4)What are the titles of the films with id 11768, 11955, 21191

SELECT title
FROM movie
WHERE id IN (11768, 11955, 21191)

5)What id number does the actress 'Glenn Close' have?
SELECT id
FROM actor
WHERE name = 'Glenn Close'

6)What is the id of the film 'Casablanca'
SELECT id
FROM movie
WHERE title = 'Casablanca'

7) Obtain the cast list for 'Casablanca'.

what is a cast list?
The cast list is the names of the actors who were in the movie.

Use movieid=11768, this is the value that you obtained in the previous question.

SELECT name
FROM actor JOIN casting ON (actor.id = casting.actorid)
WHERE movieid = 11768

8)Obtain the cast list for the film 'Alien'

SELECT name
FROM movie JOIN casting ON (movie.id = casting.movieid) JOIN actor ON (actor.id = casting.actorid)
WHERE title = 'Alien'

9) List the films in which 'Harrison Ford' has appeared
SELECT title
FROM movie JOIN casting ON (movie.id = casting.movieid) JOIN actor ON (actor.id = casting.actorid)
WHERE name = 'Harrison Ford'

10) List the films where 'Harrison Ford' has appeared - but not in the starring role. [Note: the ord field of casting gives the position of the actor. If ord=1 then this actor is in the starring role]

SELECT title
FROM movie JOIN casting ON (movie.id = casting.movieid) JOIN actor ON (actor.id = casting.actorid)
WHERE name = 'Harrison Ford' AND ord != 1

11) List the films together with the leading star for all 1962 films.

SELECT title, name
FROM movie JOIN casting ON (movie.id = casting.movieid) JOIN actor ON (actor.id = casting.actorid)
WHERE yr = 1962 AND ord = 1

12) Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
WHERE name='John Travolta'
GROUP BY yr
HAVING COUNT(title)=(SELECT MAX(c) FROM
(SELECT yr,COUNT(title) AS c FROM
   movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
 WHERE name='John Travolta'
 GROUP BY yr) AS t
)


13)List the film title and the leading actor for all of the films 'Julie Andrews' played in.



SELECT DISTINCT title, name

FROM movie JOIN casting
ON movie.id=movieid
JOIN actor
ON actorid=actor.id

WHERE movieid IN (
  SELECT movieid FROM actor JOIN casting ON (actor.id = casting.actorid)
  WHERE name='Julie Andrews') AND  ord = 1

  14) Obtain a list, in alphabetical order, of actors who've had at least 30 starring roles.

  SELECT name
FROM movie JOIN casting
  ON movie.id=movieid
  JOIN actor
  ON actorid=actor.id
WHERE ord = 1
GROUP BY 1
HAVING COUNT(ord) >= 30

15) List the films released in the year 1978 ordered by the number of actors in the cast.

SELECT DISTINCT title, COUNT(actorid) AS num_actors
FROM movie JOIN casting
  ON movie.id=movieid
  JOIN actor
  ON actorid=actor.id
WHERE yr = 1978
GROUP BY title
ORDER BY num_actors DESC

16) List all the people who have worked with 'Art Garfunkel'.

SELECT DISTINCT(name)
FROM movie JOIN casting
  ON movie.id=movieid
  JOIN actor
  ON actorid=actor.id
WHERE movieid IN
(SELECT movieid
FROM movie JOIN casting
  ON movie.id=movieid
  JOIN actor
  ON actorid=actor.id
WHERE name = 'Art Garfunkel') AND name != 'Art Garfunkel'

# SUM and COUNT

1) Show the total population of the world.

SELECT SUM(population)
FROM world

2) List all the continents - just once each.

SELECT DISTINCT continent
FROM world

3) Give the total GDP of Africa

SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa'

4) How many countries have an area of at least 1000000

SELECT COUNT(*)
FROM world
WHERE area >= 1000000

5) What is the total population of ('France','Germany','Spain')

SELECT SUM(population)
FROM world
WHERE name IN ('France', 'Germany', 'Spain')

6) For each continent show the continent and number of countries.

SELECT continent, COUNT(*)
FROM world
GROUP BY continent

7) For each continent show the continent and number of countries with populations of at least 10 million.

SELECT continent, COUNT(*)
FROM world
WHERE population >= 10000000
GROUP BY continent

8) List the continents that have a total population of at least 100 million.

SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000

# NULL

1) List the teachers who have NULL for their department.

SELECT name
FROM teacher
WHERE dept is NULL

2)Note the INNER JOIN misses the teachers with no department and the departments with no teacher.

SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)


3)Use a different JOIN so that all teachers are listed. 

SELECT teacher.name, dept.name
 FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id)

4)Use a different JOIN so that all departments are listed

SELECT teacher.name, dept.name
 FROM teacher RIGHT JOIN dept

5)Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given. Show teacher name and mobile number or '07986 444 2266'

SELECT name, COALESCE(mobile, '07986 444 2266')
FROM teacher

6)Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where there is no department.

SELECT teacher.name, COALESCE(dept.name, 'None')
 FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id)

7)Use COUNT to show the number of teachers and the number of mobile phones.

SELECT COUNT(name), COUNT(mobile)
FROM teacher

8)Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.

SELECT dept.name, COUNT(teacher.name)
FROM teacher RIGHT JOIN dept ON (teacher.dept = dept.id)
GROUP BY dept.name

9) Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2 and 'Art' otherwise.

SELECT name, CASE WHEN dept IN (1, 2) THEN 'Sci' ELSE 'Art' END
FROM teacher

10)Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise.

SELECT name, CASE WHEN dept IN (1, 2) THEN 'Sci'
WHEN dept = 3 THEN 'Art'
ELSE 'None' END
FROM teacher

11)