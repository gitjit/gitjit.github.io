---
layout: post
title: "AWS Essentials : AWS Global Infrastructure"
description: "AWS Global Infrastructure"
date:   2020-01-02
tags: [AWS]
comments: false
references: [
   "Iam : https://aws.amazon.com/iam/",
   
]
---  

In this post we are going to discuss some essential topics we need to understand about AWS Global Infrastructure. Following are the major players in AWS Infrastructure. 
<img src="../../images/2020-11-13-23-11-07.png" class="img-responsive"/>

Current numbers are as shown below.  
<img src="../../images/2020-11-13-23-10-38.png" class="img-responsive"/>

<img src="../../images/2020-11-13-22-58-52.png" class="img-responsive"/>

Each region comprises of multiple AZ's and each Az's comprises of multiple independent Data Centers connected via low latency network.
<img src="../../images/2020-11-13-23-02-07.png" class="img-responsive"/>

All Az's are connected via high bandwidth, redundant global network. 

<img src="../../images/2020-11-13-23-08-42.png" class="img-responsive"/>

As explained in the table above the regional cache location sits between the Cloud Front Origin and the edge location near customer. 

<img src="../../images/2020-11-13-23-13-26.png" class="img-responsive"/>

Given below shows the regional cache and edge locations  
<img src="../../images/2020-11-13-23-14-26.png" class="img-responsive"/>
