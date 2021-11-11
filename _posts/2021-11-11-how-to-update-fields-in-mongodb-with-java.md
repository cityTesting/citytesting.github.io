---
layout: post
title: How to update fields in mongodb with Java
categories: [Java, Mongodb]
---

In this post we are going to show how to update fields in mongodb with Java

  ```
 public void changeField(String newValue) {
        MongoCollection<Document> collection = mongoDatabase.getCollection("students");
        Bson filter = eq("Name", "Peter");
        Bson update = set("student.0.something.something.field, newValue);
        collection.updateOne(filter, update);
    }
  ```
