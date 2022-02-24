---
layout: post
title: Deserialize from json to a class
categories: [json, java]
---

How to deserialize from json to a class without using a custom deserializer.

This is our class:  
{% higjlight java %}
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Builder
@Data
@AllArgsConstructor
@NoArgsConstructor
public class OurClass {
    public String currency;
    public double value;
}
{% endhighlight %}  
  
And we have these json from a response as a String:
  
{% highlight json %}
{
    "currency": "EUR",
    "value": 288.0
}
{% endhiglight%}  
  
In order to convert it to Ourclass we can do it like this:  
{% highlight java %}import com.fasterxml.jackson.databind.ObjectMapper;
import OurClass;

  private OurClass deserializeJsonToOurClass(String jsonString) throws JsonProcessingException {
        ObjectMapper objectMapper = new ObjectMapper()
                .configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        return objectMapper.readValue(jsonString, new TypeReference<OurClass>(){});
    } {% endhighlight %}   

Note: This is possible because we are using "normal" types. I have checked that if Money from org.javamoney.moneta.Money is used does not work and it returns this error:  
> "com.fasterxml.jackson.databind.exc.InvalidDefinitionException: Cannot construct instance of org.javamoney.moneta.Money (no Creators, like default constructor, exist)"   
  
So we will need to use something similar to [this](https://citytesting.github.io/custom-json-deserializer-java/).
