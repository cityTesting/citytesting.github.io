---
layout: post
title: Custom Json java deserializer
categories: [json, deserializer, java]
---

**Custom Json java deserializer**  
```
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

```

And OurClass:  
```
import org.bson.types.ObjectId;
import com.fasterxml.jackson.databind.annotation.JsonDeserialize;
import OurClassDeserializer;

@JsonDeserialize(using = OurClassDeserializer.class)
public class OurClass {
    ObjectId id;
    String deposits;
}
```