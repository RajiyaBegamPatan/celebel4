# celebel4
1.SELECT DISTINCT CITY
FROM STATION
WHERE LOWER(LEFT(CITY, 1)) IN ('a', 'e', 'i', 'o', 'u')
  AND LOWER(RIGHT(CITY, 1)) IN ('a', 'e', 'i', 'o', 'u');

2.SELECT MAX(POPULATION) - MIN(POPULATION) AS POPULATION_DIFFERENCE
FROM CITY;

3.SELECT
    TO_CHAR(
        SQRT(
            POWER(MAX(LAT_N) - MIN(LAT_N), 2) +
            POWER(MAX(LONG_W) - MIN(LONG_W), 2)
        ),
        '999.9999'
    ) AS Distance
FROM STATION;

4.SELECT CAST(DECIMAL(
    AVG(LAT_N), 10, 4
) AS CHAR(10)) AS MEDIAN
FROM (
    SELECT LAT_N,
           ROW_NUMBER() OVER (ORDER BY LAT_N) AS row_num,
           COUNT(*) OVER () AS total_rows
    FROM STATION
) AS numbered
WHERE row_num IN (
    (total_rows + 1) / 2,
    (total_rows + 2) / 2
);


5.SELECT CITY.Name
FROM CITY
JOIN COUNTRY
  ON CITY.CountryCode = COUNTRY.Code
WHERE COUNTRY.Continent = 'Africa';

6.SELECT CITY.Name
FROM CITY
JOIN COUNTRY
  ON CITY.CountryCode = COUNTRY.Code
WHERE COUNTRY.Continent = 'Africa';


7.SELECT CASE WHEN g.grade < 8 THEN 'NULL' ELSE s.name END, g.grade, s.marks FROM students s JOIN grades g ON s.marks BETWEEN g.min_mark AND g.max_mark ORDER BY g.grade DESC, CASE WHEN g.grade < 8 THEN CHAR(s.marks) ELSE s.name END;

8.SELECT 
    s.hacker_id, 
    h.name
FROM 
    Submissions s
JOIN 
    Challenges c ON s.challenge_id = c.challenge_id
JOIN 
    Difficulty d ON c.difficulty_level = d.difficulty_level
JOIN
    Hackers h ON s.hacker_id = h.hacker_id
WHERE 
    s.score = d.score
GROUP BY 
    s.hacker_id, h.name
HAVING 
    COUNT(DISTINCT s.challenge_id) > 1
ORDER BY 
    COUNT(DISTINCT s.challenge_id) DESC,
    s.hacker_id ASC;

9.SELECT 
    w.id, 
    wp.age, 
    w.coins_needed, 
    w.power
FROM 
    Wands w
JOIN 
    Wands_Property wp ON w.code = wp.code
WHERE 
    wp.is_evil = 0
    AND w.coins_needed = (
        SELECT MIN(w2.coins_needed)
        FROM Wands w2
        JOIN Wands_Property wp2 ON w2.code = wp2.code
        WHERE wp2.is_evil = 0 
          AND wp2.age = wp.age
          AND w2.power = w.power
    )
ORDER BY 
    w.power DESC, 
    wp.age DESC;

10.SELECT 
    h.hacker_id,
    h.name,
    SUM(t.max_score) AS total_score
FROM 
    Hackers h
JOIN (
    SELECT 
        hacker_id, 
        challenge_id, 
        MAX(score) AS max_score
    FROM 
        Submissions
    GROUP BY 
        hacker_id, challenge_id
) AS t ON h.hacker_id = t.hacker_id
GROUP BY 
    h.hacker_id, h.name
HAVING 
    SUM(t.max_score) > 0
ORDER BY 
    total_score DESC,
    h.hacker_id ASC;


