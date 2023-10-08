# USBabyNames_Superset
A dashboard with some fun visuals on US Baby names given since 1880. Data is sourced from SSA url https://www.ssa.gov/oact/babynames/limits.html

Some SQL queries run in Apache Superset SQL Lab:

Capture Unique Names by Starting Alphabet

SELECT StartingLetter, UniqueNamesCount
FROM (
  SELECT LEFT(`Name`, 1) AS StartingLetter, COUNT(DISTINCT `Name`) AS UniqueNamesCount,
         RANK() OVER (ORDER BY COUNT(DISTINCT `Name`) DESC) as ranking
  FROM opendata.`namesUS`
  WHERE `Sex` = 'Female'
  GROUP BY LEFT(`Name`, 1)
) AS SubQueryMale
WHERE ranking <= 26  
ORDER BY ranking ASC;

Query to capture most popular Baby Boy Names

SELECT `Year`, `Name`, MaxOccurrences
FROM (
  SELECT `Year`, `Name`, MAX(`Occurrences`) as MaxOccurrences,
         RANK() OVER (PARTITION BY `Year` ORDER BY MAX(`Occurrences`) ASC) as ranking
  FROM opendata.`namesUS`
  WHERE `Sex` = 'Male'
  GROUP BY `Year`, `Name`
) AS SubQuery
WHERE ranking <= 20
ORDER BY `Year` ASC, ranking ASC;

Filter for baby girl names starting with the letter 'X'

SELECT `Name`
FROM opendata.`namesUS`
WHERE `Sex` = 'Male' AND SUBSTR(`Name`, 1, 1) = 'X';

![popular-baby-names-in-usa-2023-10-08T01-45-49 111Z](https://github.com/HrMav/USBabyNames_Superset/assets/132946730/4d449798-ec9e-48ca-8995-14cf2de54aab)

