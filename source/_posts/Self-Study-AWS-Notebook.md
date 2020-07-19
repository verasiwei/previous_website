---
title: Self-Study AWS Notebook
date: 2019-03-12 15:19:32
tags:
categories:
- Programming
---

### Updating......
They are all modular things, a cheatsheet to save time when I cannot remember something
## Storage

### AWS S3
Block storage: stores data organized as an array of unrelated blocks
File storage: stores data in a hierarchy of files and folders
Object storage: stores data as objects which consist of data, metadata and identifier

Buckets: Create a bucket and store data/Pay only what you use/Region based
Objects: Objects stored in buckets(image, word...) Object metadta=Name | Value
Object storage classes: S3 standard; S3 intelligent-tiering; S3 standard-IA; S3 One Zone-IA; S3 Glacier

1) insert your AWS access key ID and secret access key
```
D:\> aws configure
D:\> aws s3api list-buckets
D:\> aws s3 ls s3://
D:\> aws s3api head-object --bucket bucketname --key objectname 
```
2) S3 access permission: how to add permission of another account? https://docs.aws.amazon.com/quicksight/latest/user/using-s3-files-in-another-aws-account.html
```
#Object
#abort multipart upload
s3:AbortMultipartUpload
#delete object
s3:DeleteObject
#get object, head object, select object content
s3:GetObject

#Bucket
#get public access block
s3:GetAccountPublicAccessBlock
#put public accessblock, delete public access block
s3:PutAccountPublicAccessBlock

```
3) Create new bucket:
```
aws s3api create-bucket --bucket bucket_name
```
4) Create new objects(prefix) within bucket:
```
aws s3api put-object --bucket bucket_name --key foldername/
```
5) Check bucket size
```
aws s3 ls --summarize --human-readable --recursive s3://bucket_name/
```

## Compute

### AWS EC2
EC2 is the AWS to create and run virtual machines in the cloud, the VM is called "instances". 

To use an SSH client to connect to an linux instance
```
ssh -i /path/MyKeyPair.pem ec2-user@IP_address
```

Move data to and from S3 and EC2 instance
```
ec2-user@IPaddress $ wget https://bucket.s3.amazonaws.com/path-to-file
or
ec2-user@IPaddress $ aws s3 cp s3://bucket/path-to-file filename
or download an entire S3 bucket to a directory on the instance
ec2-user@IPaddress $ aws s3 sync s3://bucket directory_on_instance
```

## Analytics

### Amazon QuickSight
1) creating a data set using S3 files
2) creating a data set using Amazon Athena Data: https://docs.aws.amazon.com/quicksight/latest/user/create-a-data-set-athena.html
