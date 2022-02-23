---
layout: post
title: Activate the failing mode in mongodb
categories: [mongodb]
---
  
Sometimes it is interesting to simulate errors in the database to test certain cases, with the following steps we can simulate a failure in the mongodb database locally.  
We use a mongodb running in docker.  
Steps:  

- Stop and remove the mongo container

```
docker stop mongodb
docker rm mongodb
``` 

- Start it with the test commands parameters (--setParameter enableTestCommands=1)

```
docker run --name some-mongo -d mongo:tag --setParameter enableTestCommands=1
``` 

- Go into the docker container  
```
docker exec -it some-mongo bash
 ```
- Connect to mongo  

```
mongo
 ```
- Enable Fail Point

```
db.adminCommand({configureFailPoint: "failCommand",mode: "alwaysOn",data: {errorCode: 2, failCommands: ["find","delete","findAndModify","insert", "update"]}});
 ```

- Do what you want to test here  
- Disable Fail Point

```
db.adminCommand({configureFailPoint: "failCommand", mode: "off"})
 ```

More information [here](https://github.com/mongodb/mongo/wiki/The-"failCommand"-fail-point) .