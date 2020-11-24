---
title: Create S3 Bucket with the AWS CLI
description: How to create an S3 bucket using the AWS CLI
first-published: 2017-08-06
tags:
- AWS S3
- AWS CLI
---

The simplest form of using the AWS CLI to create an S3 bucket is:

```bash
aws s3 mb s3://bucket-name
```

You can optionally specify the region:

```bash
aws s3 mb s3://bucket-name --region eu-west-1
```

If you don't specify the region, the default is `us-east-1` (N. Virginia).

### IAM ###

To be able to create buckets, you need an IAM role that has at least the
following policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCreateS3Bucket",
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ]
        }
    ]
}
```

