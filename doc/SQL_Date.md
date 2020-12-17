# SQLite date 

## using the Text storage class for storing SQlite date and time 

We have a table named `datetime_text`for demonstration 

```SQL
CREATE TABLE datetime_text (
    d1 TEXT,
    d2 TEXT
);
```

And, we can insert the date and time values into the `datetime_text` table as follows:

```SQL
INSERT INTO datetime_text (d1, d2)
VALUES(datetime('now'), datetime('now', 'localtime'));
```

`datetime('now') ` can get the current UTC date and time value<br/>
`datetime('now', 'localtime')` can get the local time 

