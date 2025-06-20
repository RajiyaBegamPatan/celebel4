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

