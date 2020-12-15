# SQL Query statement 


## Use ORDER BY 



**basic format**

```SQL
SELECT 
    select_list
FROM 
    table
ORDER BY 
    column_1 ASC,
    column_2 DESC;
```


**examples**

Now, we have a `tracks` table . It stores the data of songs. Each track belongs to one album. There are some fileds on this table

- `TrackId`
    primary key
- `Name`
    The name of specific song 
- `AlbumId`
    The song belong to which album 
- `MediaTypeId`
    The type of specific song media (MPEG, AAC) 
- `GenreId`
    The music type (such as rock , jazz, etc)
- `Composer`
    The author of the song 
- `Millseconds`
    Duration of music 
- `Bytes`
    Disk space occupied by music 
- `UnitPrice`
    The price 


The following is the structure of this table.

```SQL
CREATE TABLE tracks(
    TrackId INTEGER NOT NULL PRIMARY KEY,
    Name NVARCHAR(200) NOT NULL,
    AlbumId INTEGER,
    MediaTypeId INTEGER NOT NULL, 
    GenreId INTEGER,
    Composer NVARCHAR(220),
    Milliseconds INTEGER NOT NULL,
    Bytes INTEGER,
    UnitPrice NUMERIC(10,2) NOT NULL
)
```

Suppose , we have many records on this table . And we want to get data from `name, milliseconds, and album id` columns. We can use the following statement 

```SQL
SELECT 
    name,
    milliseconds,
    albumid 
FROM 
    tracks;
```
And Now you want to sort the result based on `AlbumId` column in `ascending order`, you can use the following statement. 

```SQL
SELECT
	name,
	milliseconds, 
	albumid
FROM
	tracks
ORDER BY
	albumid ASC; 
```

And , `SQLite` uses `ASC` by default so you can omit it behind `ORDER BY  [column_name]` 

Now, Suppose you want to sort result (by `AlbumId`) above by the `Milliseconds` column in desecnding order. You can use the following statement 

```SQL
SELECT
	name,
	milliseconds, 
	albumid
FROM
	tracks
ORDER BY
	albumid ASC,
    milliseconds DESC;
```

And `SQLITE` support using `ORDER BY` with the column position.For examples , the following statement sorts the tracks by both `albumid` (3rd column) and `milliseconds` (2nd column) in ascending order. 

```SQL
SELECT
	name,
	milliseconds, 
	albumid
FROM
	tracks
ORDER BY
	 3,2;
```


## SQLite `Where` statement


**basic format**

```SQL
SELECT 
    column_list
FROM
    table 
WHERE 
    search_condition 
```

The search condition in the `WHERE` has the following form 

```
left_expresstion COMPATISON_OPERATOR right_expression 
```
For example, you can form a search condition as follows:

```
WHERE column_1 = 100;
WHERE column_2 IN (1, 2, 3);
WHERE column_3 LIKE 'An%';
WHERE column_4 BETWEEN 10 AND 20;
```

### SQLITE Comparison operators 

- `=`  
    Equal to 
- `<>` or `!=`
    Not equal to 
- `<`
    Less than 
- `>`
    Greater than 
- `<=` 
    less than or equral to 
- `>=`
    Greater than or equal to 

### SQLITE logical operators 

- `ALL`
- `AND`
- `ANY`
- `BETWEEN`
- `EXISTS`
- `IN`
- `LIKE`
- `NOT`
- `OR`

### SQLITE `WHERE` clause example 


The following picture is the structure of  `tracks` table  

![](images/tracks_table.png)


Now, we want to find all the tracks in the album id 1, we can use the following statement 

```SQL
SELECT 
    name,
    milliseconds,
    bytes,
    albumid
FROM
    tracks
WHERE 
    albumid = 1; 
```

And **We can use the logical operator to combine expressions**.For example , to get track of the album 1 that have the length greater than 250,000 millseconds,  we use the following statement 

```SQL
SELECT 
    name, 
    milliseconds,
    bytes,
    albumid
FROM
    tracks
WHERE 
    albumid = 1
AND milliseconds > 250000;

```

### SQLITE `WHERE` clause with `LIKE` operator example 

Sometimes, you may not remember exactly the data that you want to search. In this case, you perform an inexact search using the `LIKE` operator 

For example , to find which tracks composed by Simth , you use the `LIKE` operator as follows 


```SQL
SELECT 
    name,
    albumid,
    composer
FROM
    tracks
WHERE 
    composer LIKE '%Smith'
ORDER BY
    albumid;
```

### SQLITE `WHERE` clause with the `IN` operator example 

If you want to find tracks that have media type id is 2 or 3, you use the `IN` operator as shown in the following statement 

```SQL
SELECT
    name,
    albumid,
    mediatypeid,
FROM
    tracks
WHERE 
    mediatypeid IN(2,3);
```


## SQLITE Inner Join 


### Introduction to `SQLite inner join` clause 

Suppose you have two tables: A and B. 

A has a1, a2, and f columns. B has b1, b2 and f column. The A table links to the B table using a foreign key column named f. <br />
The following illustrates the syntax of the inner join clause:

```SQL
SELECT a1, a2, b1, b2 
FROM A
INNER JOIN B on B.f = A.f; 
```
For each row in the A table, the `INNER JOIN` clause compares the value of f `column` with the value of the f column in the B table. If the value of the f column in the A table equals the value of the f column in the B table, it combines data from a1, a2, b1, b2 columns and includes this row in the result set. <br />
In other words, the `INNER JOIN` clause returns rows from the A table that has the corresponding row in B table. 


### SQLite INNER JOIN examples 

Now, we have two databases, `tracks` and `albums` .The `tracks` table links to the `albums` table via `AlbumId` column <br />
In the `tracks` table , the `AlbumId` column is a foreign key. And in the `albums` table, the `AlbumId` is the primary key. <br />
To query data from both `tracks` and `albums` tables, you use the following statement 

```SQL
SELECT 
    trackid, 
    name,
    title
FROM
    tracks
INNER JOIN albums ON albums.albumid = tracks.albumid;
```

## SQLITE LIMIT 

You can use `LIMIT` clause to constrain the number of rows return by the query 

basic format 

```SQL
SELECT
	column_list
FROM
	table
LIMIT offset, row_count;

```