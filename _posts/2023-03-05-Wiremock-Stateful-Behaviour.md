---
layout: post
title: Wiremock Stateful Behaviour
categories: [wiremock, api]
---

When working with third-parties you do not always get the same response when calling the same endpoint, so it is interesting to be able to simulate these behaviours in order to test the integration of our system with them.

Wiremock has the Stateful behaviour which allows us to simulate this behaviour. To do this we have to create these two mappings as follows:
```java
curl --location --request POST 'http://localhost:8089/__admin/mappings' --header 'Content-Type: application/json' --data-raw '{  "scenarioName": "Fail second attempt", "newScenarioState":"firstAttempt", "request": {"method": "GET","url": "/invoices"},"response": {"status": 400}}'
```

```java
curl --location --request POST 'http://localhost:8089/__admin/mappings' --header 'Content-Type: application/json' --data-raw '{"scenarioName": "Fail second attempt","newScenarioState": "secondAttempt","requiredScenarioState": "firstAttempt","request": {"method": "GET","url": "/invoices"},"response": {"status": 200}}'
```
![](https://i.imgur.com/LZ5WsJI.png)

After this we will receive first a 400 and then 200.

[More info on Wiremock](https://wiremock.org/docs/stateful-behaviour/)

