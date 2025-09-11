# SQL conspect 1
A conspect for postgresql queries that will be required for lab work 1

> Note all examples are writen when logged in as user stud
> you can log in as stud using this command:
```bash
psql -h pgsql3.mif biblio -U stud
```

[databse strcuture here](https://klevas.mif.vu.lt/~baronas/dbvs/biblio/bibliosh.htm)

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
> Operators
> =
> >
> <
> >=
> <=
> <> or !=
> AND
> OR
> IN
> BETWEEN low AND high
> LIKE
> IS NULL
> NOT

Example: Select users with names 'Lukas'
```psql
SELECT vardas, pavarde, adresas
FROM skaitytojas
WHERE vardas = 'Lukas';
```

### DISTINCT
```psql
SELECT DISTINCT(field)
FROM table;
```
Returns only unique values

## [Pattern matching](https://www.postgresql.org/docs/current/functions-matching.html)
### LIKE
```psql
field LIKE 'string_%';
```
Returns bool if field is like string
> _ - wild card, any single character
> % - 0, one or more characters
> Can be combined with AND or OR
Example: Select adreses that contains two 'i' letters
```psql
SELECT vardas, pavarde, adresas 
FROM skaitytojas
WHERE adresas LIKE '%i%i%';
```
Example: Select all users and return names and bool does name contain letter 'o'
```psql
SELECT vardas, vardas LIKE '%o%'
FROM skaitytojas;
```

## [LOWER](https://www.postgresql.org/docs/9.1/functions-string.html)
Makes strings lowercase 'LabAs' => 'labas'
> Useful when comparing two strings, because 'Jonas' != 'jonas'


## [Aggregate functions](https://www.postgresql.org/docs/8.2/functions-aggregate.html)
### AVG
Returns average value
> Null values will not be counted towards will not count towards the average
### COUNT
Counts values, that are **not null**. 
> If user name is null and you query COUNT(vardas), user without a name will not be counter
> COUNT(vardas, pavarde) will count people who have a name, a surname or both, won't count people who do not have a surname or name
### MAX
Returns biggest value
### MIN
Returns smallest value
### SUM
Returns sum of values


## [Conditional Expressions](https://www.postgresql.org/docs/current/functions-conditional.html)
### CASE
```psql
SELECT CASE
    WHEN vardas
FROM skaitytojas
```
### COALESCE
### NULLIF
```psql
SELECT field
FROM table
WHERE NULLIF(field, value);
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

