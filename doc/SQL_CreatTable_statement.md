# create table 

**situations:**

Suppose you have to manage contacts , Each contact has the following information:

- First name 
- Last name 
- Email 
- Phone 

The requirement is that the `email` and `phone` must be unique. In addition, ***each contact belongs to one or many groups,*** and each group can have zero or many contacts.

Based on these requirements, we came up with threee tables.

- `contacts` table that stores contact information 
- `groups` table that stores group information 
- `contact_groups` table that stores the relationship between `contacts` and `groups` 




```SQL
CREATE TABLE contacts (
	contact_id INTEGER PRIMARY KEY,
	first_name TEXT NOT NULL,
	last_name TEXT NOT NULL,
	email TEXT NOT NULL UNIQUE,
	phone TEXT NOT NULL UNIQUE
);

CREATE TABLE groups(
    group_id INTEGER PRIMARY KEY,
    name TEXT NOT NULL
)

```

```SQL
CREATE TABLE contact_groups(
   contact_id INTEGER,
   group_id INTEGER,
   PRIMARY KEY (contact_id, group_id),
   FOREIGN KEY (contact_id) 
      REFERENCES contacts (contact_id) 
         ON DELETE CASCADE 
         ON UPDATE NO ACTION,
   FOREIGN KEY (group_id) 
      REFERENCES groups (group_id) 
         ON DELETE CASCADE 
         ON UPDATE NO ACTION
);
```