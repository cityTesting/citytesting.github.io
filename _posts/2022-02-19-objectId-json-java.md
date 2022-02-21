---
layout: post
title: Notes
categories: [mongoDb, java]
---

**Create a mongo ObjectId from a json response**  
```
ObjectId id = new ObjectId(jsonNode.get("id").asText());
```