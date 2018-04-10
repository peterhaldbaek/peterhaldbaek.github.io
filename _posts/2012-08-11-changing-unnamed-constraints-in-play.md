---
title: Changing unnamed constraints in Play!
layout: post
tags: ["sql","java","play","mysql"]
date: 2012-08-11
commentsUrl: http://www.briskbee.com/2012/08/changing-unnamed-constraints-in-play.html#comment-form
redirect_from: "/posts/changing_unnamed_constraints_in_play_/"
---

I have been working on a project based on the [Play! framework](http://www.playframework.org/) lately (the old version, [1.2.4](http://www.playframework.org/documentation/1.2.4/home)) and I came across a real headscratcher. My problem was I needed to delete a foreign constraint in a table but this constraint had no name (it was not given a name when it was created). Deleting a constraint with standard SQL requires a name so I had to do come up with something else for my evolution script. I was using [MySQL](http://www.mysql.com/) in production and [H2](http://www.h2database.com/) in test so it was not an option to use some MySQL specific code unless it was compatible with H2.

Let's make a little example first to illustrate my problem. Consider these two tables:

```
CREATE TABLE a (
    id INTEGER NOT NULL AUTO_INCREMENT,
    CONSTRAINT pk_a PRIMARY KEY(id)
);
CREATE TABLE b (
    id INTEGER NOT NULL AUTO_INCREMENT,
    a_id INTEGER NOT NULL,
    PRIMARY KEY(id),
    FOREIGN KEY(a_id) REFERENCES a(id)
);
```

Table B has a foreign constraint for the column a_id which points to the id column in the A table. If I wanted to delete the foreign constraint I should execute this statement:

```
ALTER TABLE b DROP CONSTRAINT "constraint name"
```

But since the foreign constraint was not named initially I cannot use this statement. My next thought was to find a way to list all the foreign constraints of the table and then use the result as input to an alter table statement. I did not find any way to do this with standard SQL but using some MySQL-specific SQL I actually managed to do this. The select query looks like this:

```
SELECT
    constraint_name,
    CONCAT(table_name, '.', column_name) AS 'foreign key',
    CONCAT(referenced_table_name, '.', referenced_column_name) AS 'references'
FROM
    information_schema.key_column_usage
WHERE
    referenced_table_name IS NOT NULL;
```

This lists all foreign key constraints in the database. I tried executing it in H2 and to no surprise it did not work.

I decided not to change the existing table but instead create a new table (without constraints) and copy the contents of the old table to the new table. If you need to keep the name of the table as it was you could always rename the table but since the naming of the table was a bit off in my case I actually ended up with a better named table anyway. And I am not sure renaming a table is supported by standard SQL.

So lets first create a new table without the constraints:

```
CREATE TABLE b_new (
    id INTEGER NOT NULL AUTO_INCREMENT,
    a_id INTEGER NOT NULL,
    CONSTRAINT pk_b PRIMARY KEY(id)
);
```
Then copy the data from the old to the new table:

```
INSERT INTO b_new SELECT * FROM b ORDER BY id;
```
The last thing to do is to delete the old table (and maybe renaming the new one):

```
DROP TABLE b;
```

That's it. Now the table has no foreign constraints.

So if you need a reason for why you always should name your constraints, now you know why.
