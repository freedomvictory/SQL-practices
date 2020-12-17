# SQL Foreign key 

## SQLite foreign key constraint support 

You can use the following command to check whether you current version of `SQLite` support foreign key constraints or not.

```SQL
PRAGMA foreign_keys;
```
If there was somthing returns. It assume your SQlite support 
`foreign key constraints`. If it returns 1 , it assume SQlite foreign key `enable`. If it return 0, it .. `disable`.

And you can use the following command , enable or disable foreign key constraints at runtime. 

```SQL
PRAGMA foreign_keys = ON;  //enable
PRAGMA foreign_keys = OFF; //disable 
```

## Instroduction `SQLite foreign key constraints`

Now we have two tables. one is `suppliers` another is `supplier_groups`. And each `supplier` belongs to one and only one `supplier group`.And each `supplier group` may have zero or `many suppliers`.<br/>
Yes, the relationship between `supplier_groups` and `suppliers` table is `one-to-many`.In other words , for each row in the `suppliers` table, there is a corresponding row in the `supplier_groups` table.<br/>


To enforce the relationship between rows in the `suppliers` and `supplier_groups` table, you use the **foreign key constraints** 

```SQL
CREATE TABLE supplier_groups (
	group_id integer PRIMARY KEY,
	group_name text NOT NULL
);
CREATE TABLE suppliers (
    supplier_id   INTEGER PRIMARY KEY,
    supplier_name TEXT    NOT NULL,
    group_id      INTEGER NOT NULL,
    FOREIGN KEY (group_id)
       REFERENCES supplier_groups (group_id) 
);

```
The `supplier_groups` table is called a **parent table**, which is the table that a foreign key references. The `supplier` table is known as a **child table**, which is the table to which the foreign key constraint applies.

The `group_id` column in the `supplier_groups` table is called the **parent key**, which is a column or a set of columns in the parent table that foreign key constraint references. Typically, the parent key is the primary key of the parent table. 

The `group_id` column in the `suppliers` table is called the **child key**. Generally, the child key references to the primary key of the parent table. 





## SQLite foreign key constraint example  

Now ,we insert some contents to our `supplier_groups` table  


```SQL
INSERT INTO supplier_groups (group_name)
VALUES
    ('Domestic'),
    ('Global'),
    ('One-Time');  
```

So that , the `supplier_groups` table will become like this 

| Group_id      | group_name    | 
| ------------- |:-------------:| 
| 1             | Domestic      | 
| 2             | Global        |  
| 3             | One-Time      |    

And then, we insert a new supplier into the `supplier` table with the `supplier group` that exists in the `supplier_groups` table. We use the following SQL statement. 

```SQL
INSERT INTO supplier( supplier_name, groupid)
VALUES ('HP', 2);
```
It works perfectly fine. <br/>
But if we insert a new supplier into the `supplier` table with the `supplier group` that doesn't exist in the `supplier_groups` table . It will get some error. 

```SQL
INSERT INTO suppliers (supplier_name, group_id)
VALUES('ABC Inc.', 4);
```
The error message is  

```
[SQLITE_CONSTRAINT]  Abort due to constraint violation (FOREIGN KEY constraint failed)
```

## SQLite foreign key constraint actions 


What would happen if you delete a row in the `supplier_groups` table, Should all the corresponding rows in the `supplier` table are alse deleted? or do others ? How about update? 

So that , we could specify how foreign key constraint behaves whenever the parent key is deleted or updated. We use  `ON DELETE`, `ON UPDATE` action as follows 

```SQL
FOREIGN KEY (foreign_key_colums)
    REFERENCES parent_table(parent_key_columns)
        ON UPDATE action
        ON DELETE action; 
```
SQLite suppors the following actions : 

- SET NULL 
- SET DEFAULT 
- RESTRICT 
- NO ACTION 
- CASCADE 

Let's focus on `CASCADE`, others please refer [it](https://www.sqlitetutorial.net/sqlite-foreign-key/)

The `CASCADE` performance is : **When deleting or updating a row of the parent table, some rows or one row correspongding to the child table(the row associated with the deletion or update of the parent table ) will be deleted**


Now, we drop and recreated supplier table. 
```SQL
DROP TABLE suppliers;
CREATE TABLE suppliers (
    supplier_id   INTEGER PRIMARY KEY,
    supplier_name TEXT    NOT NULL,
    group_id      INTEGER,
    FOREIGN KEY (group_id)
    REFERENCES supplier_groups (group_id) 
       ON UPDATE CASCADE
       ON DELETE CASCADE
);
```
And we insert the some rows into the `supplier_groups` table 

```SQL 
INSERT INTO supplier_groups (group_name)
VALUES
   ('Domestic'),
   ('Global'),
   ('One-Time');
```

So that the `supplier_groups` table will become like this. 

| Group_id      | group_name    | 
| ------------- |:-------------:| 
| 1             | Domestic      | 
| 2             | Global        |  
| 3             | One-Time      |    

Then , we insert some supplier into the table `suppliers`

```SQL
INSERT INTO suppliers (supplier_name, group_id)
VALUES('XYZ Corp', 1);

INSERT INTO suppliers (supplier_name, group_id)
VALUES('ABC Corp', 2);
```

| Supplier_name | group_id      | 
| ------------- |:-------------:| 
| XYZ Corp      | 1             | 
| ABC Corp      | 2             |  

Now we delete supplier group id 2 from the `supplier_groups` table 

```SQL
DELETE FROM supplier_groups 
WHERE group_id = 2;
```
Finally , we query data from the table `suppliers`

You will got : The supplier whose group_id is 2 was deleted . This is the effect of the `ON DELETE CASCADE` action 



