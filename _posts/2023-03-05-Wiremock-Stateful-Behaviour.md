---
layout: post
title: Wiremock Stateful Behaviour
categories: [wiremock, api]
---

When working with third-party APIs, it's not uncommon to receive different responses when calling the same endpoint. This can make it difficult to test the integration of our system with theirs. To simulate these behaviors and test our system's response to them, we can use WireMock.

WireMock has a stateful behavior that allows us to simulate these scenarios. To do this, we need to create two mappings. The first mapping sets the scenario name and new scenario state to "Fail second attempt" and "firstAttempt", respectively. It also sets the request method to GET and the URL to "/invoices". The response status is set to 400.

```java
curl --location --request POST 'http://localhost:8089/__admin/mappings' --header 'Content-Type: application/json' --data-raw '{  "scenarioName": "Fail second attempt", "newScenarioState":"firstAttempt", "request": {"method": "GET","url": "/invoices"},"response": {"status": 400}}'
```
The second mapping sets the scenario name to "Fail second attempt", new scenario state to "secondAttempt", and required scenario state to "firstAttempt". It also sets the request method to GET and URL to "/invoices". The response status is set to 200.


```java
curl --location --request POST 'http://localhost:8089/__admin/mappings' --header 'Content-Type: application/json' --data-raw '{"scenarioName": "Fail second attempt","newScenarioState": "secondAttempt","requiredScenarioState": "firstAttempt","request": {"method": "GET","url": "/invoices"},"response": {"status": 200}}'
```

With these two mappings, we can add the scenario displayed in the image below to WireMock. When we request localhost:8089/invoices for the first time, we'll receive a 400 status. When we request it again, we'll receive a 200 status.


![](https://i.imgur.com/LZ5WsJI.png)

WireMock also has other features that make it useful for testing APIs. For example, it can be used to simulate network latency or errors in responses. It also supports matching requests based on various criteria such as headers or query parameters.


[More info on Wiremock](https://wiremock.org/docs/stateful-behaviour/)

