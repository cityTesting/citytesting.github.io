---
layout: post
title: Custom Json java deserializer
categories: [json, deserializer, java]
---
We will show two ways to deserialize:
- Deserializing the whole class.  
- Deserializing by type.
  
**Deserialize whole class:**  

{% highlight java %} 
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.databind.DeserializationContext;
import com.fasterxml.jackson.databind.JsonDeserializer;
import com.fasterxml.jackson.databind.JsonNode;
import org.bson.types.ObjectId;
import java.io.IOException;

public class OurClassDeserializer extends JsonDeserializer<OurClass> {
    @Override
    public OurClass deserialize(JsonParser parser, DeserializationContext ctxt) throws IOException {
        JsonNode node = parser.getCodec().readTree(parser);
        
        ObjectId id = new ObjectId(node.get("id").asText());
        String name = node.get("name").asText();
        
        return new OurClass("id", "name");
    }
}

{% endhighlight %}

And OurClass:  
{% highlight java %}
import org.bson.types.ObjectId;
import com.fasterxml.jackson.databind.annotation.JsonDeserialize;
import OurClassDeserializer;

@JsonDeserialize(using = OurClassDeserializer.class)
public class OurClass {
    ObjectId id;
    String deposits;
}
{% endhighlight %}

**Deserialize an ObjectId and Money**  
- ObjectId:      
{% highlight java %}
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.databind.DeserializationContext;
import com.fasterxml.jackson.databind.JsonDeserializer;
import org.bson.types.ObjectId;

import java.io.IOException;

public class ObjectIdDeserializer extends JsonDeserializer<ObjectId> {
    @Override
    public ObjectId deserialize(JsonParser parser, DeserializationContext ctxt) throws IOException {
        return new ObjectId(parser.getValueAsString());
    }
}

{% endhighlight %}  
    
- Money:  

{% highlight java %}
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.databind.DeserializationContext;
import com.fasterxml.jackson.databind.JsonDeserializer;
import com.fasterxml.jackson.databind.JsonNode;
import org.javamoney.moneta.Money;

import java.io.IOException;

public class MoneyDeserializer extends JsonDeserializer<Money> {

    @Override
    public Money deserialize(JsonParser parser, DeserializationContext ctxt) throws IOException {
        JsonNode node = parser.getCodec().readTree(parser);
        return Money.of(
                Float.valueOf(node.get("value").asText()),
                node.get("currency").asText());
    }
}

{% endhighlight %}

And OurClass:  
{% highlight java %}
import com.fasterxml.jackson.databind.annotation.JsonDeserialize;
MoneyDeserializer;
ObjectIdDeserializer;
import lombok.AccessLevel;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.NoArgsConstructor;
import lombok.Value;
import org.bson.types.ObjectId;
import org.javamoney.moneta.Money;

@Value
@Builder
@NoArgsConstructor(force = true, access = AccessLevel.PRIVATE)
@AllArgsConstructor
public class OurClass {
    @JsonDeserialize(using = ObjectIdDeserializer.class) ObjectId id;
    @JsonDeserialize(using = MoneyDeserializer.class) Money money;
}
{% endhighlight %}

