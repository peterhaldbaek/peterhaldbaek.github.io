---
title: Setting character set to UTF-8 in MySQL
layout: post
tags: ["mysql","database"]
date: 2012-10-25
commentsUrl: http://www.briskbee.com/2012/10/setting-character-set-to-utf-8-in-mysql.html#comment-form
redirect_from: "/posts/setting_character_set_to_utf8_in_mysql/"
---

The default installation of MySQL (5.5) does not use UTF-8 so every time I install an instance of MySQL I spend some time tinkering with the character setup so I decided to write a quick summary of the steps needed in order for MySQL to support UTF-8.  When you have a vanilla installation of MySQL you can check your character settings by executing these commands.  

```
mysql> show variables like 'char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+

mysql> show variables like 'collation%';
+----------------------+-------------------+
| Variable_name        | Value             |
+----------------------+-------------------+
| collation_connection | utf8_general_ci   |
| collation_database   | latin1_swedish_ci |
| collation_server     | latin1_swedish_ci |
+----------------------+-------------------+
```

As we can see latin1 is used in several places. This should be changed to UTF-8. You can do this by either editing your existing my.cnf configuration file or creating a new configuration file in the conf.d folder of your MySQL configuration (recommended). I create a new configuration file at conf.d/utf8.cnf which looks like this.  

```
[mysqld]
character-set-server = utf8
character-set-client = utf8
```

Restart the server.

```
> sudo service mysql restart
```

Log in to MySQL and check that everything is ok.  

```
mysql> show variables like 'char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+

mysql> show variables like 'collation%';
+----------------------+-----------------+
| Variable_name        | Value           |
+----------------------+-----------------+
| collation_connection | utf8_general_ci |
| collation_database   | utf8_general_ci |
| collation_server     | utf8_general_ci |
+----------------------+-----------------+
```

Everything is now using UTF-8.  

## Update 21.11.2012

If you want to use utf8_unicode_ci instead of utf8_general_ci (you probably
want that, see
[this](http://stackoverflow.com/questions/766809/whats-the-difference-between-utf8-general-ci-and-utf8-unicode-ci))
then add the following to the my.cnf file in the mysqld section and restart the
server.  

```
collation-server = utf8_unicode_ci
```

## Update 21.11.2012 (a little later)

The collation settings should be something like this.

```
mysql> show variables like 'collation%';
+----------------------+-----------------+
| Variable_name        | Value           |
+----------------------+-----------------+
| collation_connection | utf8_general_ci |
| collation_database   | utf8_unicode_ci |
| collation_server     | utf8_unicode_ci |
+----------------------+-----------------+
```
I previously had the impression that it should be <code>utf8_unicode_ci</code>
for all collation variables (obtained by setting the
<code>skip-character-set-client-handshake</code> option in my.cnf) but this
generated weird behavior when comparing dates in the database.
