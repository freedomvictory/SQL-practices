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







