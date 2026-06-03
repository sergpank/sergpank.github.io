---
layout: post
title: AWS CodePipeline trigger fix
date: 2026-06-02
categories: AWS
---

 - Somebody disabled `Data Events` ?
  - CloudTrail -> Insights -> Data Events
    - YES !
 - They want to save some $$$ for company!
 - But my deployments don't run, what shall I do!
 - Don't panic! Follow these steps:

```bash
1. Enable [Amazon EventBridge] in S3 bucket properties
   - Send notifications to Amazon EventBridge for all events in this bucket -> ON
2. Change Event pattern for trigger:

from 
{
  "source": ["aws.s3"],
  "detail-type": ["AWS API Call via CloudTrail"],
  "detail": {
    "eventSource": ["s3.amazonaws.com"],
    "eventName": ["PutObject", "CompleteMultipartUpload", "CopyObject"],
    "requestParameters": {
      "bucketName": ["your-bucket-name"],
      "key": ["your-object-key"]
    }
  }
}

to
{
  "source": ["aws.s3"],
  "detail-type": ["Object Created"],
  "detail": {
    "bucket": {
      "name": ["your-bucket-name"]
    },
    "object": {
      "key": ["your-object-key"]
    }
  }
}
```
