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
find . -type d -name .svn -print0 | xargs -0 rm -rf</pre>
```
### Find and sort Files in past 1 hour ###
```
find . -type f -mmin +60  -exec ls -1rtls "{}" +</pre>
```
### Find and delete in past 1 hour ###
```
find . -type f -mmin +60  -exec rm -rf {} \</pre>
```