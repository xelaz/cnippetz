cnippetz
=========


## CMD ##

### Zip & Tar ###
```
tar -cvf - * | gzip > *.tar.gz

tar xfvz *.tar.gz
```
### Search ###
```
find /home -type f -name *.txt | xargs grep "keyword"
```
### Remove SVN files recursive ###
```
find . -type d -name .svn -print0 | xargs -0 rm -rf
```
### Find and sort Files in past 1 hour ###
```
find . -type f -mmin +60  -exec ls -1rtls "{}" +
```
### Find and delete in past 1 hour ###
```
find . -type f -mmin +60  -exec rm -rf {} \
```
### sendmail test ###
```
echo -e "Subject: Test" | /usr/bin/sendmail -v your@address.tld
```


## MySql ##
### Backup All Structur+Data Databases ###
```
# mysqldump -u$USER -h$SERVER --password=$PASSWORD --opt --all-databases > backup.sql
```
### Backup All Structur Databases ###
```
# mysqldump -d -u$USER -h$SERVER --password=$PASSWORD --opt --all-databases > backup.sql
```
### Backup Table Structur+Data from Database ###
```
# mysqldump -u$USER -h$SERVER --password=$PASSWORD --opt --databases $SHEMA --tables $TABLE > table.sql
```
### Backup Table Structur from Database ###
```
# mysqldump -d -u$USER -h$SERVER --password=$PASSWORD --opt --databases $SHEMA --tables $TABLE > table.sql
```
### Import MySql Dump ###
```
# mysql -u$USER -p$PASSWORD -h$SERVER $shema < backup.sql
```
### Change Root Password ###
```
# mysqladmin -u$USER password NEWPASSWORD
#or
$mysql> update user set password=PASSWORD("NEWPASSWORD") where User='root';
```

### Enable/Disable Foreign Key Checks ###
```
# disable foreign key checks
$mysql> SET FOREIGN_KEY_CHECKS = 0;
#enable the foreign key checks
$mysql> SET FOREIGN_KEY_CHECKS = 1;
```

### Get Random Rows ###
```
SELECT t.* FROM table as t,
(SELECT ROUND((SELECT MAX(id) FROM table) *rand()) as rnd FROM test LIMIT 100) tmp
WHERE t.id in (rnd)
```

### Profiling ###
```
SET profiling=1;
SELECT * FROM table;
SHOW profiles;
SHOW profile for query ?;
SHOW profile cpu for query ?;
SET profiling=0;
```

### Query optimizing ###
```
$mysql> EXPLAIN EXTENDED SELECT * FROM table WHERE id = 12345
$mysql> SHOW WARNINGS;
```

### Reset root user ###
```
cat > restore_root_privileges.sql

update mysql.user set Super_priv='y' where user='root';
update mysql.user set Select_priv='y' where user='root';
update mysql.user set Insert_priv='y' where user='root';
update mysql.user set Update_priv='y' where user='root';
update mysql.user set Delete_priv='y' where user='root';
update mysql.user set Create_priv='y' where user='root';
update mysql.user set Drop_priv='y' where user='root';
update mysql.user set Reload_priv='y' where user='root';
update mysql.user set Shutdown_priv='y' where user='root';
update mysql.user set Process_priv='y' where user='root';
update mysql.user set File_priv='y' where user='root';
update mysql.user set Grant_priv='y' where user='root';
update mysql.user set References_priv='y' where user='root';
update mysql.user set Index_priv='y' where user='root';
update mysql.user set Alter_priv='y' where user='root';
update mysql.user set Show_db_priv='y' where user='root';
update mysql.user set Super_priv='y' where user='root';
update mysql.user set Create_tmp_table_priv='y' where user='root';
update mysql.user set Lock_tables_priv='y' where user='root';
update mysql.user set Execute_priv='y' where user='root';
update mysql.user set Repl_slave_priv='y' where user='root';
update mysql.user set Repl_client_priv='y' where user='root';
update mysql.user set Create_view_priv='y' where user='root';
update mysql.user set Show_view_priv='y' where user='root';
update mysql.user set Create_routine_priv='y' where user='root';
update mysql.user set Alter_routine_priv='y' where user='root';
update mysql.user set Create_user_priv='y' where user='root';

/etc/init.d/mysqld stop

/etc/init.d/mysqld startsos

# OR

mysqld_safe --skip-grant-tables &

mysql -vv < restore_root_privileges.sql
/etc/init.d/mysql restart
mysql -u root -p
```

### FLUSH PRIVILEGES ###
```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
quit;
```

### Get Table ForeignKey Dependency ###
```
SELECT
  TABLE_NAME,
  COLUMN_NAME,
  CONSTRAINT_NAME,
  REFERENCED_TABLE_NAME,
  REFERENCED_COLUMN_NAME
 FROM
  INFORMATION_SCHEMA.KEY_COLUMN_USAGE
 WHERE
  REFERENCED_TABLE_NAME = "my_table";
```