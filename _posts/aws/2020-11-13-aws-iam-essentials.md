---
layout: post
title: "AWS Essentials : Identity and Access Management(IAM)"
description: "AWS Identity and Access Management"
date:   2020-11-13
tags: [AWS]
comments: false
references: [
   "Iam : https://aws.amazon.com/iam/",
   
]
---  

In this post we are going to discuss some essential we need to know about AWS IAM, Users and Policy.  

## IAM Roles  
Let us consider you have a Lambda function that needs to access some records in a DynamoDb table. By default Lambda does not have access to the DynamoDb records. In order for Lambda to access the DynamoDb records, we need to assign a "role" to the Lambda function. The role should have a policy attached to it which specifies the the permission to access the records.  

## AWS Security Token Service (STS)  
The AWS Security Token Service (STS) is a web service that enables you to request temporary, limited-privilege credentilas for IAM users or users that you authenticate(federated users.)
The AWS Security Token Service (STS) is a web service that enables you to request temporary
