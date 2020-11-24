---
title: Upload file to S3 Bucket using Boto3
description: How to upload a file to an S3 bucket using Boto3
first-published: 2017-08-07
tags:
- AWS S3
- Boto3
---

How to upload files to S3 using Boto3, either with the high-level s3 resource or the low-level s3 client.

<!-- read more -->

### Using the High-Level S3 Resource ###

```python
import boto3

data = open('test.txt', 'rb')
s3 = boto3.resource('s3')
s3.Bucket('bucket-name').put_object(Key='test.txt', Body=data)
```

The last statement returns an `s3.Object` instance for the file.

### Using the Low-Level S3 Client ###

```python
import boto3
data = open('test.txt', 'rb')
s3 = boto3.client('s3')
s3.put_object(Bucket='bucket-name', Key='test.txt', Body=data)
```

The response of the above call will be something like:

```python
{
    u'ETag': '"123412349d1bbd07abcd65819a54321"', 
    'ResponseMetadata': {
        'HTTPStatusCode': 200, 
        'RetryAttempts': 0, 
        'HostId': 'yYQugBzOogwz23XET6V5/7KbS+R+cuEY/MQzUcyHoyXzJqaQLc5eK3/RkNiBsXTXyJSyLmasubI=', 
        'RequestId': '04511C835A020E21', 
        'HTTPHeaders': {
            'content-length': '0', 
            'x-amz-id-2': 'yYQugBzOogwz23XET6V5/7KbS+R+cuEY/MQzUcyHoyXzJqaQLc5eK3/RkNiBsXTXyJSyLmasubI=', 
            'server': 'AmazonS3', 
            'x-amz-request-id': '04511C835A020E21', 
            'etag': '"126a8a51b9d1bbd07fddc65819a542c3"', 
            'date': 'Mon, 07 Aug 2017 15:00:36 GMT'
        }
    }
}
```

### Upload String as File ###

Sometimes you will have a string that you want to save as an S3 Object. Since
the SDK methods require a file-like object, you can convert the string to that
form with either `StringIO` (in Python2) or `io` (in Python3). For example, in
Python2:

```python
import boto3
import StringIO

my_string = 'Something I want to save on S3.'
data = StringIO.StringIO(my_string)
s3 = boto3.resource('s3')
s3.Bucket('bucket-name').put_object(Key='test.txt', Body=data)
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
