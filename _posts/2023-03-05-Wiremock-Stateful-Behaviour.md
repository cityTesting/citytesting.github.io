---
layout: post
title: Wiremock Stateful Behaviour
categories: [wiremock, postman]
---

When working with third-parties you do not always get the same response when calling the same endpoint, so it is interesting to be able to simulate these behaviours in order to test the integration of our system with them.

Wiremock has the Stateful behaviour which allows us to simulate this behaviour. To do this we have to create these two mappings as follows:
{% highlight java %}curl --location --request POST 'http://localhost:8089/__admin/mappings' \
--header 'Content-Type: application/json' \
--data-raw '{
  "scenarioName": "Fail second attempt",
  "newScenarioState":"firstAttempt",
  "request": {
    "method": "GET",
    "url": "/invoices"
  },
  "response": {
    "status": 400
  }
}'{% endhighlight %}

{% highlight java %}curl --location --request POST 'http://localhost:8089/__admin/mappings' \
--header 'Content-Type: application/json' \
--header 'Cookie: RMC=mNFdg0IcdgdMXT3RCAseGkgEOmYMbX9.1g7u2n613.de_DE' \
--data-raw '{
  "scenarioName": "Fail second attempt",
  "newScenarioState": "secondAttempt",
  "requiredScenarioState": "firstAttempt",
  "request": {
    "method": "GET",
    "url": "/invoices"
  },
  "response": {
    "status": 200
  }
}'{% endhighlight %}

After this we will receive first a 400 and then 200.

[More info on Wiremock](https://wiremock.org/docs/stateful-behaviour/)
