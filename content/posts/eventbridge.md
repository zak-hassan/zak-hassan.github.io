---
title: "Building Event Driven Infrastructure with EventBridge"
date: 2021-07-10T15:00:18-07:00
draft: false
---

## Purpose

Eventbridge used to be called cloudWatch Events. I believe custom events do cost money however AWS events are free as long as its in the same account. Sometimes when you build applications you might request resources that you don't use. If your use case is that you want to trigger something to happen only when an event occurs in your system then AWS EventBridge might be for you. It is essentially a eventbus which captures all events in your aws account. It also has a schema registry which can let you generate code bindings. Based on an event it can trigger a lambda function or SNS topic which will do some processing. Serverless architectures will save money and resources. Imagine having something run 24/7 will cause you to get charged by the hour. For example it could cost you anywhere from   $18.36/month for (a1.medium) - $239.61/month (t3.2xlarge) per hour. Regardless what instance type you use you may not be using all the resources and maybe putting this to waste.


## Features

- You can create custom events and have it managed by AWS
- When you create an event bridge rule there is a source or target.
- SNS/SQS automatically scales and you can use lambda concurrency setting to scale it.
- Eventbridge soft limit 400 putEvents and 750 target invocations. But you can increase this limit.
- With Eventbridge it can read messages based on schema and do conditional filtering. 
- You can store all schema in a schema registry.
- It can also generate a code biding to directly read and process the message
- It can automatically discover schemas


## Example of Rule

The example below shows a rule that will trigger event bus to send an event when `example.property` is changed in System Manager Parameter Store. 

```json
{
    "source": [
        "aws.ssm"
    ],
    "detail-type": [
        "Example Eventbridge Rule"
    ],
    "detail": {
        "name": [
            "example.property"
        ],
        "operation": [
            "Update"
        ]
    }
}

```