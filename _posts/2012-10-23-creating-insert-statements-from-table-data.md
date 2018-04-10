---
title: Creating insert statements from table data
layout: post
tags: ["sql","mysql","database"]
date: 2012-10-23
commentsUrl: http://www.briskbee.com/2012/10/creating-insert-statements-from-table.html#comment-form
redirect_from: "/posts/creating_insert_statements_from_table_data/"
---

Creating a SQL script which inserts data from an already existing table is not as straightforward as one would think. I found a pretty clean and easy solution which I will explain in detail here. I use MySQL but I believe the principles can be applied to most other databases if needed.  Consider table A with the following content.  

| Id | Name  |
|----|-------|
| 1  | Peter |
| 2  | Jake  |
| 3  | Paul  |

I want to create a script inserting all entries where name starts with P. The first step is to create a table containing these entries.

```
CREATE TABLE b (SELECT FROM a WHERE name LIKE 'P%');
```
Now the data can be exported using mysqldump.  

```
> mysqldump -p -u <username> <database> b > insert-script.sql
```

The created script contains statements which will drop and create the table (among other things). Normally I am only interested in the part where the insert statements are so open the created script file and remove everything except the <code>INSERT INTO</code> statement. Also remember to rename the name of the table where the data is to be inserted to the correct table name (table A in my case).  The final script file should look something like this.  

```
INSERT INTO a VALUES (1, 'Peter'),('3', 'Paul');
```

Finally you should drop the temporary table you created.
