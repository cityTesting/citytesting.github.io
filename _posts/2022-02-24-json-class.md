---
layout: post
title: Deserialize from json to a class
categories: [json, java]
---

How to deserialize from json to a class without using a custom deserializer.

{% highlight java %}      
import com.fasterxml.jackson.databind.ObjectMapper;
import OurClass;

  private OurClass deserializeJsonToOurClass(String jsonString) throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper()
                .configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        return objectMapper.readValue(jsonString, new TypeReference<OurClass>(){});
    }  
    {% endhighlight %}   

Note: This is possible because we are using "normal" types. I have checked that if Money from org.javamoney.moneta.Money is used does not work and it returns this error:  
> "com.fasterxml.jackson.databind.exc.InvalidDefinitionException: Cannot construct instance of `org.javamoney.moneta.Money` (no Creators, like default constructor, exist)"   
So we will need to use something similar to [this](https://citytesting.github.io/custom-json-deserializer-java/).
