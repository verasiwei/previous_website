---
title: Cheatsheet Shell Scripts
date: 2019-01-04 15:04:51
tags:
categories:
- Programming
---

## Something useful for shell scripts/gawk scripts/git commands/aws commands

### create public ssh key

```
$ ssh-keygen -t rsa -C "siwei"
$ cat ~/.ssh/id_rsa.pub

```

### connect to the remote server and modify the files on the server remotely

```
#download rmate firstly
$ ssh -R 52698:127.0.0.1:52698 username@server 
$ rmate -p 52698 filename
```

### change group of files

```
#change all files under this folder
$ chgrp -R group file
```

### change permission

```
#have a look of the permission of a folder/file
$ getfacl "file"

#change the permission of a user read/write/execute
$ setfacl -m u:userID:rwx "file"

#change the permission of a group read/write/execute 
$ setfacl -m g:groupID:rwx "file"
```

### deal with gzipped files

```
#have a look of compressed files
$ zcat file | head

#uncompress files
$ gunzip -c file > /directory/file

#compress files
$ gzip -c file > /directory/file 
```

### loop

```
$for i in `seq 1 22`;do
gawk -v chr=${i} '$5=="SNP" {print chr" "$2}' >> file
done
```

### ifelse

```
$ if ["answer" == "local"]; then
for i in `seq 1 22`;do
echo "hello world" >> file1
done
else
echo "hello world" >> file2
fi
```

### git commands

```
#one local repository to multiple remote repositories
$ git remote add both repository1
$ git remote remote set-url --add --push both repository1
$ git remote set-url --add --push both repository2
$ git add .
$ git commit -m first_commit
$ git push both
```

### aws commands
Reference: from AWS tutorial
Amazon S3 is a repository for internet data. It is designed to store and retrieve any amount of data from Amazon EC2. Amazon EC2 uses Amazon S3 for storing Amazon Machine Images(AMIs), users use AMI for launching EC2 instances. 

```
#to get started
$ aws configure
#list the contents in S3
$ aws s3api list-buckets

```



