---
layout: post
title: How to update fields in mongodb with Java
categories: [Java, Mongodb]
---


  ```
  public class MongoClient{
    private final MongoDatabase mongoDatabase;

      public WalletMongoClient() {
          com.mongodb.MongoClient mongoClient = new com.mongodb.MongoClient(
                  new MongoClientURI(
                          "StringURLtoMongodb"
                  )
          );

          this.mongoDatabase = mongoClient.getDatabase("databaseName );
      }
   public void changeField(String newValue) {
          MongoCollection<Document> collection = mongoDatabase.getCollection("students");
          Bson filter = eq("Name", "Peter");
          Bson update = set("student.0.something.something.field, newValue);
          collection.updateOne(filter, update);
      }
}
  ```
