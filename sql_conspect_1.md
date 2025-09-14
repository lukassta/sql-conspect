# SQL conspect 1
A conspect for postgresql queries that will be required for lab work 1

> Note: all examples are writen when logged in as user stud\
you can log in as stud using this command:
```bash
psql -h pgsql3.mif biblio -U stud
```

[database structure here](https://klevas.mif.vu.lt/~baronas/dbvs/biblio/bibliosh.htm)

## [Select](https://www.postgresql.org/docs/current/sql-select.html)
### SELECT
Basic select statement
```psql
SELECT field
FROM table;
```

### WHERE
```psql
SELECT field
FROM table
WHERE condition;
```
> Operators:\
=\
\>\
<\
\>=\
<=\
<> or !=\
AND\
OR\
IN\
BETWEEN low AND high\
LIKE\
IS NULL\
NOT

> Example: Select users with names 'Lukas'
```psql
SELECT vardas, pavarde, adresas
FROM skaitytojas
WHERE vardas = 'Lukas';
```

### DISTINCT
Returns only unique values
```psql
SELECT DISTINCT(field)
FROM table;
```

### [AS](https://www.w3schools.com/postgresql/postgresql_as.php)
Can rename column
```psql
SELECT field as "Alternate field name"
FROM table;
```

## [Value Expressions](https://www.postgresql.org/docs/8.3/sql-expressions.html)
### CAST
Specifies a conversion from one data type to another ('::' is a historical way to write cast)
```psql
CAST(value AS TYPE)
```

## [Pattern matching](https://www.postgresql.org/docs/current/functions-matching.html)
### LIKE
Returns bool if field is like string
```psql
value LIKE 'string_%'
```
> _ - wild card, any single character\
% - 0, one or more characters\
Can be combined with AND or OR

> Example: Select adreses that contains two 'i' letters
```psql
SELECT vardas, pavarde, adresas 
FROM skaitytojas
WHERE adresas LIKE '%i%i%';
```
> Example: Select all users and return names and bool does name contain letter 'o'
```psql
SELECT vardas, vardas LIKE '%o%'
FROM skaitytojas;
```
## [Date/Time functions and operators](https://www.postgresql.org/docs/current/functions-datetime.html)
### EXTRACT
Exctracts specific time unit froma DATE or a TIMESTAMP
```psql
EXTRACT(time_unit FROM value)
```
> Possible time units:\
CENTURY\
DAY\
DECADE\
DOW (day of week sunday(0) saturday(7))\
DOY (day of year 1–365/366)\
EPOCH\
HOUR\
ISODOW (day of week monday(1) sunday(7))\
ISOYAR (week-numbering year that the date falls in)\
JULIAN (Julian date)\
MICROSECONDS\
MILLENIUM\
MILLISECONDS\
MINUTE\
MONTH\
QUARTER\
SECOND\
TIMEZONE\
TIMEZONE_HOUR\
TIMEZONE_MINUTE\
WEEK\
YEAR

## [String functions and operators](https://www.postgresql.org/docs/9.1/functions-string.html)
### LOWER
Makes strings lowercase 'LabAs' => 'labas'
```psql
LOWER(value)
```
> Useful when comparing two strings, because 'Jonas' != 'jonas'


## [Aggregate functions](https://www.postgresql.org/docs/8.2/functions-aggregate.html)
### AVG
Returns average value
```psql
SELECT AVG(field)
FROM table;
```
> Null values will not be counted towards the average

### COUNT
Counts values, that are **not null**.
```psql
SELECT COUNT(field)
FROM table;
```
> `COUNT(vardas)` counts the number of rows where `vardas IS NOT NULL`. `COUNT(vardas, pavarde)` is equivalent to `COUNT(ROW(vardas, pavarde))` and since a row is considered to be `NULL` iff all of its fields are `NULL`, `COUNT(vardas, pavarde)` counts rows where `vardas IS NOT NULL OR pavarde IS NOT NULL`. In other words, if at least one value in the row `IS NOT NULL`, the row will be counted.

### MAX
Returns biggest value
```psql
MAX(value)
```

### MIN
Returns smallest value
```psql
MIN(value)
```

### SUM
Returns sum of values
```psql
SUM(value)
```

## [Conditional expressions](https://www.postgresql.org/docs/current/functions-conditional.html)
### CASE
Generic conditional expressions (if else in programming), that returns the value where the first condition is met or if none conditions are met return the ELSE value
```psql
CASE
    WHEN condition1 THEN value1
    WHEN condition2 THEN value2
    WHEN condition3 THEN value3
    ELSE value4
END
```
> Example, get the day of week when the books needs to be returned
```psql
SELECT egzempliorius,
CASE
        WHEN EXTRACT(DOW FROM grazinti) = 1 THEN 'Pirmadieni'
        WHEN EXTRACT(DOW FROM grazinti) = 2 THEN 'Antradieni'
        WHEN EXTRACT(DOW FROM grazinti) = 3 THEN 'Treciadieni'
        WHEN EXTRACT(DOW FROM grazinti) = 4 THEN 'Ketvirtadieni'
        WHEN EXTRACT(DOW FROM grazinti) = 5 THEN 'Penktadieni'
        WHEN EXTRACT(DOW FROM grazinti) = 6 THEN 'Sestadieni'
        WHEN EXTRACT(DOW FROM grazinti) = 7 THEN 'Sekmadieni'
END AS "Grazinti savaites diena"
FROM skaitymas;
```

### COALESCE
Returns first value that is not null, if all values are null returns null
```psql
COALESCE(value1, value2 [, ...])
```
> Example, get average of book price where null values are counted as 0
```psql
SELECT AVG(COALESCE(verte, 0))
FROM knyga;
```

### NULLIF
Returns null if argument values match
```psql
NULLIF(value1, value2)
```
> Example, do not return users, who have entered their name as vardenis
```psql
SELECT vardas
FROM skaitytojai
WHERE NULLIF(LOWER(vardas), 'vardenis');
```
> Example, count people whose name is not Lukas
```psql
SELECT COUNT(NULLIF(LOWER(vardas), 'lukas'))
FROM skaitytojai,
```

## [Mathematical functions and operators](https://www.postgresql.org/docs/8.1/functions-math.html)
### ABS
Gives absolute value
```psql
ABS(value)
```

### ROUND
Gives rounded value
```psql
ROUND(value)
```
