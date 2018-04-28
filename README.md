<!--
title: AWS Fetch image from URL and upload to S3 example in NodeJS
description: This example display how to fetch an image from remote source (URL) and then upload this image to a S3 bucket.
layout: Doc
-->
# Fetch court data from URL then upload to s3 Example

This function fetches data from court finder and upload the data to a S3 bucket. The `serverless.yaml` specifies when the function runs, the data URL and the key suffix to use when saving the data. 

## How it works

We first fetch the data from given url and then call the S3 API `putObject` to upload it to the bucket.

```js
fetch('image URL')
  .then(res => {
    return s3.putObject({Bucket, Key, Body: res.body}).promise();
  }).then(res => {
    callback(null, res);
  }).catch(err => {
    callback(err, null);
  });
```

## Setup

You need to create an S3 bucket you want to store the files in. After you created the bucket, change the bucket name in `serverless.yml` custom settings to your buckets.

```yml
custom:
  bucket: <your-bucket-name>
```

## Deploy

In order to deploy your endpoint simply run

```bash
serverless deploy
```

## Usage

You can now send an HTTP request directly to the endpoint using a tool like curl

```bash
serverless invoke --function save --log --data='{ "data_url": "https://courttribunalfinder.service.gov.uk/courts.json", "key_suffix": "court.json"}'
```

The expected result should be similar to:

```bash
{
    "ETag": "\"1fc0f55f4c7832dc60f0b3bb9f496db5\""
}
--------------------------------------------------------------------
START RequestId: 53075ac4-4aef-11e8-b974-e5a83c9209d3 Version: $LATEST
END RequestId: 53075ac4-4aef-11e8-b974-e5a83c9209d3
REPORT RequestId: 53075ac4-4aef-11e8-b974-e5a83c9209d3  Duration: 352.59 ms     Billed Duration: 400 ms         Memory Size: 1024 MB    Max Memory Used: 52 MB
```

## Scaling

By default, AWS Lambda limits the total concurrent executions across all functions within a given region to 100. The default limit is a safety limit that protects you from costs due to potential runaway or recursive functions during initial development and testing. To increase this limit above the default, follow the steps in [To request a limit increase for concurrent executions](http://docs.aws.amazon.com/lambda/latest/dg/concurrent-executions.html#increase-concurrent-executions-limit).
