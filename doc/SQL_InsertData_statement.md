# SQL insert data into table statement 



## Insert a single row into a table 

**basic format** 

```SQL
INSERT INTO table(column1, column2, ...)
VALUES(value1, value2, ....);
```

There was a example. We have a `artists` table , and we need to insert a new row data.


```SQL
CREATE TABLE artists(
    artist_id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL
);

INSERT INTO artists (name)
    VALUES('Bud Powell');
```

And then we could query data , using the following SQL statement 

```SQL
SELECT
    artist_id,
    name
FROM
    Artists
ORDER BY
    artist_id DESC
LIMIT 1;
```

## Insert multiple rows into a table  

**basic format**

```SQL
INSERT INTO table1 (column1,column2 ,..)
VALUES 
   (value1,value2 ,...),
   (value1,value2 ,...),
    ...
   (value1,value2 ,...);
```

Example: 
We could insert 3 rows data into the above table `artists`

```SQL
INSERT INTO artists (name)
VALUES
	("Buddy Rich"),
	("Candido"),
	("Charlie Byrd");
```

And then , we could use the following SQL statement to query these three rows data

```SQL
SELECT
	ArtistId,
	Name
FROM
	artists
ORDER BY
	ArtistId DESC
LIMIT 3;
```

## Inserting new rows with data provided by `SELECT` statement 


**situations**

Now you want to backup the `artists` table , you can follow these steps:

First, create a new table named `artists_backup` as follows 

```SQL
CREATE TABLE artists_backup(
    ArtistId INTEGER PRIMARY KEY AUTOINCREMENT,
    Name NVARCHAR
)
```
Then, To insert data into `artists_backup` table with the data from the `artists` table , you use the `INSERT INTO SELECT` statement as follows:

```SQL
INSERT INTO artists_backup
SELECT ArtistId, Name 
FROM artists;
```
And query data 

```SQL
SELECT * FROM artists_backup
```






