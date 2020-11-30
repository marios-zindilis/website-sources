---
title: Create S3 Bucket with Boto3
description: How to use Boto3 to create an S3 bucket.
first-published: 2017-08-06
tags:
- AWS S3
- Boto3
---

How to use Boto3 to create S3 buckets.

<!-- read more -->

### Using the High-Level S3 Resource ###

The simplest way to create a bucket using Boto3 is:

```python
import boto3
s3 = boto3.resource('s3')
s3.create_bucket(Bucket='bucket-name')
```

The `create_bucket` method takes a few parameters, but only `Bucket` is
mandatory. You can optionally specify the region:

```python
import boto3
s3 = boto3.resource('s3')
s3.create_bucket(
    Bucket='a-bucket-made-in-ireland',
    CreateBucketConfiguration={
        'LocationConstraint': 'eu-west-1'})
```

If you don't specify the region, the default is `us-east-1` (N. Virginia).

### Using the Low-Level S3 Client ###

In this scenario, the behaviour of the low-level client is the same as that of
the high-level resource, except that the client returns a dictionary that
contains the response from the S3 API:

```python
import boto3
s3 = boto3.client('s3')
response = s3.create_bucket('bucket-name')
```

The response of the above call will be something like:

```python
{
    u'Location': '/bucket-name',
    'ResponseMetadata': {
        'HTTPStatusCode': 200,
        'RetryAttempts': 0,
        'HostId': 'abcdefghijklmnopqrstu1233563467LUFVWzDlEdy4zQGfM4QAmFoOp/p1oiU0hNmac=',
        'RequestId': '6AECB9651A3D5D72',
        'HTTPHeaders': {
            'content-length': '0',
            'x-amz-id-2': 'eswnQrmQbue4njDNVlgkkN4lI9UrN1+Cmnm2waLUFVWzDlEdy4zQGfM4QAmFoOp/p1oiU0hNmac=',
            'server': 'AmazonS3',
            'x-amz-request-id': '6AECB9651A3D5D72',
            'location': '/bucket-name',
            'date': 'Sun, 06 Aug 2017 14:56:37 GMT'
        }
    }
}
```

### Using a Pre-Signed URL ###

You can generate a pre-signed URL for creating a bucket, and call it:

```python
import boto3
import requests

s3 = boto3.client('s3')
url = s3.generate_presigned_url('create_bucket', Params={'Bucket': 'bucket-name'})
response = requests.put(url)
```

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
