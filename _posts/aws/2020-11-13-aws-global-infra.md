---
layout: post
title: "AWS Essentials : AWS Global Infrastructure"
description: "AWS Global Infrastructure"
date:   2020-01-01
tags: [AWS]
comments: false
references: [
   "Iam : https://aws.amazon.com/iam/",
   
]
---  

In this post we are going to discuss some essential topics we need to understand about AWS Global Infrastructure. Following are the major players in AWS Infrastructure. 
![Iam](../../images/2020-11-13-23-11-07.png){:class="img-responsive centerimg" :max-width="500px"} 

Current numbers are as shown below.  
![Iam](../../images/2020-11-13-23-10-38.png){:class="img-responsive centerimg" :height="500px" width="500px"} 

<img src="../../images/2020-11-13-22-58-52.png" class="img-responsive" width="50%">
![](../../images/2020-11-13-22-58-52.png)

Each region comprises of multiple AZ's and each Az's comprises of multiple independent Data Centers connected via low latency network.

![](../../images/2020-11-13-23-02-07.png)

All Az's are connected via high bandwidth, redundant global network. 

![](../../images/2020-11-13-23-08-42.png)

As explained in the table above the regional cache location sits between the Cloud Front Origin and the edge location near customer. 

![](../../images/2020-11-13-23-13-26.png)


<img src="../../images/2020-11-13-23-14-26.png" class="img-responsive" width="100%">
