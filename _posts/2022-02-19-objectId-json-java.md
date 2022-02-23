---
layout: post
title: Create a mongo ObjectId from a json response
categories: [mongodb, java]
---

**Create a mongo ObjectId from a json response**  

<% highlight java %>
ObjectId id = new ObjectId(jsonNode.get("id").asText());
<% endhighlight %>
