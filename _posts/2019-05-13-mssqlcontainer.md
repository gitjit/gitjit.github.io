---
layout: post
title:  "MSSql Linux Container"
description: "Setting up MSSql linux container on Mac"
date:   2019-05-13
tags: [Docker]
comments: false
references: [
   "https://github.com/Microsoft/mssql-docker/issues/315",
   "https://hub.docker.com",
]

excerpt: "This article shows step to configure an MSSql docker container in Mac."
---

Recently I was trying to follow a tutorial on EFCore that actively uses MSSql. I was 
using a Mac and hence decided to use Container.  Following are the steps I followed.

Ensure you have installed docker for mac. you can download it from here.   

https://docs.docker.com/v17.12/docker-for-mac/install/

I am using the image : mssql-server-linux:2017-latest.

Now run the following docker command.  

```bash

 docker run --name sql_server_demo -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=1Secure*Password1' -e 'MSSQL_PID=Enterprise' -p 1433:1433 -d microsoft/mssql-server-linux:2017-latest

```  
Ensure that you are specifying a secure password that meets the SQL server criteria. I had issues in getting this instance up and running due to password criteria acceptance.  

Now let us test our MSSql server by interacting with container OS.  

```bash
docker exec -it sql_server_demo /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P 1Secure*Password1

```

Now we should get the sqlcmd and try out following commands. 

```sql
>CREATE DATABASE TestDB
>SELECT Name from sys.Databases  
```
The previous two commands were not executed immediately. You must type GO on a new line to execute the previous commands.  

If everything goes well you should have an MSSql running in your Mac. 

https://gist.github.com/Untrusted-Game/80022c4c4b30d9e96818658c4f3bb5d9. 


Enjoy coding... !






