---
title: Upload file to S3 Bucket with the AWS CLI
description: How to upload a file to an S3 bucket using the AWS CLI
first-published: 2017-08-07
tags:
- AWS S3
- AWS CLI
---

The simplest form of using the AWS CLI to upload a file to an S3 bucket is:

```
aws s3 cp test.txt s3://bucket-name
```

A neat trick is that you can pipe the output of a shell command to a file on
S3, for example:

```
find . | aws s3 cp - s3://bucket-name/test.txt
```

### IAM ###

To be able to upload files, you need an IAM role that has at least the
following policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowUploadFileToS3Bucket",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::bucket-name/*"
            ]
        }
    ]
}
```
