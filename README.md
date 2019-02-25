cnippetz
=========


## CMD ##

### Zip & Tar ###
```bash
tar -cvf - * | gzip > *.tar.gz
tar xfvz *.tar.gz
```

### Mac OS remove @ right's
```
xattr -cr ${FOLDER}
```
### Search Files ###
```bash
find /home -name *PATTERN*
```
### Search in Files ###
```bash
find /home -type f -name *.txt | xargs grep "keyword"
grep -rnw '/path/to/somewhere/' -e 'pattern'
```
### Remove SVN files recursive ###
```bash
find . -type d -name .svn -print0 | xargs -0 rm -rf
```
### Find and sort Files in past 1 hour ###
```bash
find . -type f -mmin +60  -exec ls -1rtls "{}" +
```
### Find and delete in past 1 hour ###
```bash
find . -type f -mmin +60  -exec rm -rf {} \
```
### sendmail test ###
```bash
echo -e "Subject: Test" | /usr/bin/sendmail -v your@address.tld
```
### check user permission on folder ###
```bash
sudo -u www-data stat /path/to/folder/
```
### Add a new user to the www-data group
```
sudo useradd -g www-data myuser
### set the password for myuser ###
sudo passwd myuser
```
### Add a existing user to www-data group
```
sudo usermod -a -G www-data  myuser
id myuser
groups myuser
```
### www-data supplementary groups membership
```
sudo groupadd myuser
sudo useradd -g vivek -G www-data,ftp myuser
sudo passwd myuser
```

### Swap File ###
```bash
swapon -s

dd if=/dev/zero of=/swapfile1 bs=1024 count=524288
chown root:root /swapfile1
chmod 0600 /swapfile1
mkswap /swapfile1
swapon /swapfile1
echo "/swapfile1 none swap sw 0 0" >> /etc/fstab

free -m
```
### Rename recursive File Extensions
```
find . -name '*.jade' | xargs rename 's/\.jade$/.pug/'
```

## GIT ##
### Add submodule and checkout tag ###
```bash
git submodule add git//project.git new/path
git commit -m"new submodule"
git push

# go in submodule if you want checkout specific tag or branch of submodule
cd new/path
# check your tags
git tag -l
git checkout $tag
git commit -m'new tag'
# go back to project root
cd ..
# update submodule to new tag
git submodule update --merge --rebase
```

### Remove Submodule ###
```bash
submodulePath="vendor/example"
git config -f .git/config --remove-section "submodule.${submodulePath}"
git config -f .gitmodules --remove-section "submodule.${submodulePath}"
git rm --cached "${submodulePath}"
rm -rf "${submodulePath}"
rm -rf ".git/modules/${submodulePath}"
git add .gitmodules
git commit -m "Removed ${submodulePath}"
```

## Docker ##
### Remove all images and containers
```bash
# Kill all container
docker kill $(docker ps -q)
# Delete all containers
docker rm $(docker ps -a -q)
# Delete all images
docker rmi $(docker images -q)
# Backup image
docker save mage/name | gzip -c > image.tgz
# Delete all stopped containers
docker rm $(docker ps -q -f status=exited)
# Delete all dangling (unused) images
docker rmi $(docker images -q -f dangling=true)
# get ssh connection to docker
docker exec -it ${DOCKER_ID} bash
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
```sql
# disable foreign key checks
SET FOREIGN_KEY_CHECKS = 0;

#enable the foreign key checks
SET FOREIGN_KEY_CHECKS = 1;
```

### Get Random Rows ###
```sql
SELECT t.* FROM table as t,
(SELECT ROUND((SELECT MAX(id) FROM table) *rand()) as rnd FROM test LIMIT 100) tmp
WHERE t.id in (rnd)
```

### Profiling ###
```sql
SET profiling=1;
SELECT * FROM table;
SHOW profiles;
SHOW profile for query ?;
SHOW profile cpu for query ?;
SET profiling=0;
```

### Query optimizing ###
```sql
EXPLAIN EXTENDED SELECT * FROM table WHERE id = 12345
SHOW WARNINGS;
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
```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
quit;
```

### Get Table ForeignKey Dependency ###
```sql
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

## Apache ##

### Permalinks mod_rewrite ###
```apacheconfig
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} -s [OR]
  RewriteCond %{REQUEST_FILENAME} -l [OR]
  RewriteCond %{REQUEST_FILENAME} -d
  RewriteRule ^.*$ - [NC,L]
  RewriteRule ^.*$ index.php [NC,Lo]
</IfModule>
```

### Https mod_rewrite ###
```apacheconfig
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{HTTPS} !=on
  RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</IfModule>
```

### WWW mod_rewrite ###ect
```apacheconfig
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{HTTP_HOST} ^www\.(.+)$ [NC]
  RewriteRule ^(.*)$ https://%1$1 [R=301,L]
</IfModule>
```

## Nginx ##

### Enable Multisite Sub directory language in Wordpress ###
```nginxconfig
server {
    listen 80;
    server_name domain.dev;
    
    access_log {PATH_LOG_FILE}/domain.dev_access.log;
    error_log {PATH_LOG_FILE}/domain.dev_error.log error;

    root   {DOC_PATH}/;
    index  index.php index.html;

    if (!-e $request_filename) {
        rewrite /wp-admin$ $scheme://$host$uri/ permanent;
        rewrite ^(/[^/]+)?(/wp-.*) $2 last;
        rewrite ^/[^/]+(/.*.php)$ $1 last;
    }

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ ^\/(en)(\/?.*)$ {
        try_files $2 $2/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
}
```
