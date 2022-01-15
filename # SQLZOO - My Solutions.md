# SQLZOO - My Solutions

## 0 SELECT basics

1.
```
SELECT population FROM world
WHERE name = 'Germany'
```
2.
```
SELECT name, population FROM world
WHERE name IN ('Sweden', 'Norway', 'Denmark')
```
3.
```
SELECT name, area FROM world
WHERE area BETWEEN 200000 AND 250000
```

## 1 SELECT name

1.
```
SELECT name FROM world
WHERE name LIKE 'Y%'
```
2.
```
SELECT name FROM world
WHERE name LIKE '%y'
```
3.
```
SELECT name FROM world
WHERE name LIKE '%x%'
```
4.
```
SELECT name FROM world
WHERE name LIKE '%land'
```
5.
```
SELECT name FROM world
WHERE name LIKE 'C%' AND name LIKE '%ia'
```
6.
```
SELECT name FROM world
WHERE name LIKE '%oo%'
```
7.
```
SELECT name FROM world
WHERE name LIKE '%a%a%a%'
```
8.
```
SELECT name FROM world
WHERE name LIKE '_t%'
ORDER BY name
```
9.
```
SELECT name FROM world
WHERE name LIKE '%o__o%'
```
10.
```
SELECT name FROM world
WHERE name LIKE '____'
```
11.
```
SELECT name
FROM world
WHERE name=capital
```
12.
```
SELECT name
FROM world
WHERE capital=CONCAT(name,' City')
```
13.
```
SELECT capital, name
FROM world
WHERE capital LIKE CONCAT('%',name,'%')
```
14.
```
SELECT capital, name
FROM world
WHERE capital LIKE CONCAT(name,'%') AND capital != name
```
15.
```
SELECT name, REPLACE(capital,name,'') AS extension
FROM world
WHERE capital LIKE CONCAT(name,'%') AND capital != name
```

## 2 SELECT from World

1.
```
SELECT name, continent, population FROM world
```
2.
```
SELECT name FROM world
WHERE population >= 200000000
```
3.
```
SELECT name, gdp/population FROM world
WHERE population >= 200000000
```
4.
```
SELECT name, population/1000000 FROM world
WHERE continent = 'South America'
```
5.
```
SELECT name, population FROM world
WHERE name IN ('France', 'Germany', 'Italy');
```
6.
```
SELECT name FROM world
WHERE name LIKE 'United%'
```
7.
```
SELECT name, population, area FROM world
WHERE area > 3000000 OR population > 250000000
```
8.
```
SELECT name, population, area FROM world
WHERE area > 3000000 XOR population > 250000000
```
9.
```
SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000, 2) FROM world
WHERE continent = 'South America'
```
10.
```
SELECT name, ROUND(gdp/population, -3) FROM world
WHERE gdp >= 1000000000000
```
11.
```
SELECT name, capital FROM world
WHERE LENGTH(name) = LENGTH(capital)
```
12.
```
SELECT name, capital FROM world
WHERE LEFT(name,1) = LEFT(capital,1) AND name <> capital
```
13.
```
SELECT name FROM world
WHERE name LIKE '%a%' AND name LIKE '%e%' AND name LIKE '%i%' AND name LIKE '%o%' AND name LIKE '%u%' AND name NOT LIKE '% %'
```

## 3 SELECT from Nobel

1.
```
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1950
```
2.
```
SELECT winner
FROM nobel
WHERE yr = 1962
AND subject = 'Literature'
```
3.
```
SELECT yr, subject
FROM nobel
WHERE winner = 'Albert Einstein' 
```
4.
```
SELECT winner
FROM nobel
WHERE subject = 'Peace' AND yr >= 2000
```
5.
```
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'Literature' AND yr >= 1980 AND yr <= 1989
```
6.
```
SELECT * FROM nobel
WHERE winner IN ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter', 'Barack Obama')
```
7.
```
SELECT winner
FROM nobel
WHERE winner LIKE 'John%'
```
8.
```
SELECT yr, subject, winner
FROM nobel
WHERE subject = 'Physics' AND yr = 1980 OR subject = 'Chemistry' AND yr = 1984
```
9.
```
SELECT yr, subject, winner
FROM nobel
WHERE yr = 1980 AND (subject <> 'Chemistry' AND subject <> 'Medicine')
```
10.
```
SELECT yr, subject, winner
FROM nobel
WHERE (subject = 'Medicine' AND yr < 1910) OR (subject = 'Literature' AND yr >= 2004)
```
11.
```
SELECT *
FROM nobel
WHERE winner = 'PETER GRÜNBERG'
```
12.
```
SELECT *
FROM nobel
WHERE winner = 'EUGENE O''NEILL'
```
13.
```
SELECT winner, yr, subject
FROM nobel
WHERE winner LIKE 'Sir%'
```
14.
```
SELECT winner, subject
FROM nobel
WHERE yr=1984
ORDER BY subject IN ('Physics','Chemistry'),subject,winner
```

## 4 SELECT within SELECT

1.
```
SELECT name
FROM world
WHERE population > (SELECT population
					FROM world
					WHERE name IN ('Russia'))
```
2.
```
SELECT name
FROM world
WHERE continent IN ('Europe') AND (gdp/population)>(SELECT (gdp/population)
			FROM world
			WHERE name IN ('United Kingdom'))
```
3.
```
SELECT name, continent
FROM world
WHERE continent IN (SELECT continent
		FROM world
		WHERE name IN ('Argentina', 'Australia'))
ORDER BY name
```
4.
```
SELECT name, population
FROM world
WHERE population > (SELECT population
					FROM world
					WHERE name='Canada')
AND
	population < (SELECT population
					FROM world
					WHERE name='Poland')
```
5.
```
SELECT name, CONCAT(ROUND((population/(SELECT population FROM world WHERE name='Germany'))*100,0),'%')
FROM world
WHERE continent='Europe'
```
6.
```
SELECT name
FROM world
WHERE gdp > ALL(SELECT gdp FROM world WHERE continent='Europe' AND gdp>0)
```
7.
```
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
```
8.
```
SELECT x.continent, x.name
FROM world x
WHERE x.name <= ALL(SELECT y.name from world y WHERE x.continent=y.continent)
```
9.
```
SELECT name, continent, population
FROM world
WHERE continent IN 
(SELECT continent
FROM world x
WHERE
(SELECT MAX(population) FROM world y
WHERE x.continent = y.continent) <= 25000000);
```
10.
```
SELECT name, continent FROM world x
WHERE population > ALL
(SELECT 3*population FROM world y
WHERE x.continent = y.continent AND x.name <> y.name)
```

## 5 SUM and COUNT

1.
```
SELECT SUM(population)
FROM world
```
2.
```
SELECT continent FROM world
GROUP BY continent
```
3.
```
SELECT sum(gdp) FROM world
WHERE continent = 'Africa'
```
4.
```
SELECT count(name) FROM world
WHERE area >= 1000000
```
5.
```
SELECT sum(population) FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
```
6.
```
SELECT continent, COUNT(name)
FROM world
GROUP BY continent
```
7.
```
SELECT continent, COUNT(name)
FROM world
WHERE population >= 10000000
GROUP BY continent
```
8.
```
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000
```

## 6 JOIN

1.
```
SELECT matchid, player FROM goal 
WHERE teamid = 'GER'
```
2.
```
SELECT id,stadium,team1,team2
FROM game
WHERE id = 1012
```
3.
```
SELECT player, teamid, stadium, mdate
FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'
```
4.
```
SELECT team1, team2, player
FROM game JOIN goal ON (game.id=goal.matchid)
WHERE player LIKE 'Mario%'
```
5.
```
SELECT player, teamid, coach, gtime
FROM goal JOIN eteam ON (teamid=id)
WHERE gtime<=10
```
6.
```
SELECT mdate, teamname
FROM game JOIN eteam ON (game.team1=eteam.id)
WHERE coach = 'Fernando Santos'
```
7.
```
SELECT player
FROM goal JOIN game ON (goal.matchid=game.id)
WHERE game.stadium = 'National Stadium, Warsaw'
```
8.
```
SELECT DISTINCT(player)
  FROM game JOIN goal ON matchid = id 
    WHERE (team1='GER' OR team2='GER') AND teamid != 'GER'
```
9.
```
SELECT teamname, COUNT(teamid)
FROM eteam JOIN goal ON (eteam.id=goal.teamid)
GROUP BY teamname
```
10.
```
SELECT stadium, COUNT(matchid)
FROM game JOIN goal ON (game.id=goal.matchid)
GROUP BY stadium
```
11.
```
SELECT matchid, mdate, COUNT(matchid) AS '# Goals'
FROM game JOIN goal ON (game.id=goal.matchid)
WHERE (team1='POL' OR team2='POL')
GROUP BY matchid, mdate
```
12.
```
SELECT matchid, mdate, COUNT(matchid)
FROM game JOIN goal ON (game.id=goal.matchid)
WHERE (team1='GER' OR team2='GER') AND teamid='GER'
GROUP BY matchid, mdate
```
13.
```
SELECT mdate, team1,
SUM(CASE WHEN teamid=team1
THEN 1
ELSE 0
END) AS score1,
team2,
SUM(CASE WHEN teamid=team2
THEN 1
ELSE 0
END) AS score2
FROM game LEFT JOIN goal ON (game.id=goal.matchid)
GROUP BY mdate, matchid, team1, team2
```

## 7 More JOIN Operations

1.
```
SELECT id, title
FROM movie
WHERE yr=1962
```
2.
```
SELECT yr
FROM movie
WHERE title='Citizen Kane'
```
3.
```
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr
```
4.
```
SELECT DISTINCT actorid
FROM casting JOIN actor ON (casting.actorid=actor.id)
WHERE name = 'Glenn Close'
```
5.
```
SELECT id
FROM movie
WHERE title='Casablanca'
```
6.
```
SELECT name
FROM actor JOIN casting ON (actor.id=casting.actorid)
WHERE movieid=11768
```
7.
```
SELECT name
FROM actor JOIN casting ON (actor.id=casting.actorid)
WHERE movieid = (SELECT movieid
FROM casting JOIN movie ON (casting.movieid=movie.id)
WHERE title = 'Alien'
GROUP BY movieid)
```
8.
```
SELECT title
FROM movie JOIN casting ON (movie.id=casting.movieid)
WHERE actorid = (SELECT actorid
FROM casting JOIN actor ON (casting.actorid=actor.id)
WHERE name = 'Harrison Ford'
GROUP BY actorid)
```
9.
```
SELECT title
FROM movie JOIN casting ON (movie.id=casting.movieid)
WHERE actorid = (SELECT actorid
FROM casting JOIN actor ON (casting.actorid=actor.id)
WHERE name = 'Harrison Ford'
GROUP BY actorid) AND ord != 1
```
10.
```
SELECT movie.title, actor.name
FROM casting
INNER JOIN movie ON (movie.id=casting.movieid)
INNER JOIN actor ON (actor.id=casting.actorid)
WHERE movie.yr=1962 AND casting.ord = 1
```
11.
```
SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
        JOIN actor   ON actorid=actor.id
WHERE name='Rock Hudson'
GROUP BY yr
HAVING COUNT(title) > 2
```
12.
```
SELECT title, name
FROM movie
JOIN casting ON (movie.id=movieid)
JOIN actor ON (actor.id=actorid)
WHERE ord = 1 AND movieid IN
(SELECT movieid
FROM casting
JOIN actor ON (actor.id=actorid)
WHERE actor.name='Julie Andrews')
```
13.
```
SELECT name
FROM movie
JOIN casting ON (movie.id=movieid)
JOIN actor ON (actor.id=actorid)
WHERE ord=1
GROUP BY name
HAVING COUNT(name) >= 15
```
14.
```
SELECT title, COUNT(actorid) as cast
FROM movie
JOIN casting ON (movie.id=movieid)
WHERE yr=1978
GROUP BY title
ORDER BY cast DESC, title
```
15.
```
SELECT name
FROM actor
JOIN casting ON (actor.id=actorid)
WHERE movieid IN
(SELECT movieid
FROM movie
JOIN casting ON (movie.id=movieid)
JOIN actor ON (actor.id=actorid)
WHERE name = 'Art Garfunkel') AND name != 'Art Garfunkel'
```

## 8 Using NULL

1.
```
SELECT name
FROM teacher
WHERE dept IS NULL
```
2.
```
SELECT teacher.name, dept.name
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id)
```
3.
```
SELECT teacher.name, dept.name
FROM teacher
LEFT JOIN dept ON (teacher.dept=dept.id)
```
4.
```
SELECT teacher.name, dept.name
FROM teacher
RIGHT JOIN dept ON (teacher.dept=dept.id)
```
5.
```
SELECT teacher.name, COALESCE(mobile,'07986 444 2266') AS Phone
FROM teacher
```
6.
```
SELECT teacher.name, COALESCE(dept.name,'None') AS dept
FROM teacher
LEFT JOIN dept ON (teacher.dept=dept.id)
```
7.
```
SELECT COUNT(id), COUNT(mobile)
FROM teacher
```
8.
```
SELECT dept.name, COUNT(teacher.name)
FROM teacher
RIGHT JOIN dept ON (teacher.dept=dept.id)
GROUP BY dept.name
```
9.
```
SELECT teacher.name, CASE
WHEN dept=1
THEN 'Sci'
WHEN dept=2
THEN 'Sci'
ELSE 'Art'
END
FROM teacher
```
10.
```
SELECT teacher.name, CASE
WHEN dept IN (1,2)
THEN 'Sci'
WHEN dept=3
THEN 'Art'
ELSE 'None'
END AS Dept
FROM teacher
```

## 8+ Numeric Examples

1.
```
SELECT A_STRONGLY_AGREE
  FROM nss
 WHERE question='Q01'
   AND institution='Edinburgh Napier University'
   AND subject='(8) Computer Science'
```
2.
```
SELECT institution, subject
  FROM nss
 WHERE question='Q15'
   AND score >= 100
```
3.
```
SELECT institution, score
  FROM nss
 WHERE question='Q15'
   AND score < 50
   AND subject='(8) Computer Science'
```
4.
```
SELECT subject,SUM(response)
  FROM nss
 WHERE question='Q22'
   AND subject IN ('(8) Computer Science','(H) Creative Arts and Design')
GROUP BY subject
```
5.
```
SELECT subject,SUM((A_STRONGLY_AGREE*response) / 100)
  FROM nss
 WHERE question='Q22'
   AND subject IN ('(8) Computer Science','(H) Creative Arts and Design')
GROUP BY subject
```
6.
```
SELECT subject, ROUND(SUM(((response * A_STRONGLY_AGREE) / 100)) / SUM(response) * 100,0)
FROM nss
 WHERE question='Q22'
   AND (subject='(8) Computer Science' OR subject='(H) Creative Arts and Design')
GROUP BY subject
```
7.
```
SELECT institution,ROUND(SUM(score*response) / sum(response),0)
  FROM nss
 WHERE question='Q22'
   AND (institution LIKE '%Manchester%')
GROUP BY institution
ORDER BY institution
```
8.
```
SELECT institution,sum(sample),
(SELECT sample
FROM nss y
WHERE subject='(8) Computer Science'
AND x.institution = y.institution
AND question='Q01') AS comp
FROM nss x
 WHERE question='Q01'
   AND (institution LIKE '%Manchester%')
GROUP BY institution
```

## 9- Window Function

1.
```
SELECT lastName, party, votes
FROM ge
WHERE constituency='S14000024' and yr=2017
ORDER BY votes DESC
```
2.
```
SELECT party, votes,
       RANK() OVER (ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000024' AND yr = 2017
ORDER BY party
```
3.
```
SELECT yr,party, votes,
      RANK() OVER (PARTITION BY yr ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000021'
ORDER BY party,yr
```
4.
```
SELECT constituency,party,votes,
RANK() OVER (PARTITION BY constituency ORDER BY votes DESC) AS posn
  FROM ge
 WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
   AND yr  = 2017
ORDER BY posn, constituency
```
5.
```
SELECT constituency, party
FROM (SELECT constituency, party, votes,
RANK() OVER (PARTITION BY constituency ORDER BY votes DESC) AS posn
FROM ge
WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
AND yr  = 2017
ORDER BY posn, constituency) x
WHERE x.posn = 1
```
6.
```
SELECT party, count(*)
FROM (SELECT constituency, party
FROM (SELECT constituency, party, votes,
RANK () OVER (PARTITION BY constituency ORDER BY votes DESC) AS posn
FROM ge
WHERE constituency LIKE 'S%'
AND yr = 2017
ORDER BY posn, constituency) x
WHERE x.posn=1) y
GROUP BY party
```

## 9+ COVID-19

1.
```
SELECT name, DAY(whn),
confirmed, deaths, recovered
FROM covid
WHERE name = 'Spain'
AND MONTH(whn) = 3 AND YEAR(whn) = 2020
ORDER BY whn
```
2.
```
SELECT name, DAY(whn), confirmed,
LAG(confirmed, 1) OVER (PARTITION BY name ORDER BY whn) AS DayBefore
FROM covid
WHERE name = 'Italy'
AND MONTH(whn) = 3 AND YEAR(whn) = 2020
ORDER BY whn
```
3.
```
SELECT name, DAY(whn), confirmed -
LAG(confirmed, 1) OVER (PARTITION BY name ORDER BY whn)
FROM covid
WHERE name = 'Italy'
AND MONTH(whn) = 3 AND YEAR(whn) = 2020
ORDER BY whn
```
4.
```
