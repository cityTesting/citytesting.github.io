---
layout: post
title: Create a Kafka consumer to test a kafka producer
categories: [kafka, java]
---

When working with Kafka it is very convenient to prove that we are producing the message correctly, if we are the producer.  That is why we are going to show how to do this with Java. For this we are going to create a producer which will consume a message of the topic "topic_name".


{% highlight java %} private KafkaMessage getKafkaMessage() {
        ConsumerRecord<String, KafkaMessage> record = null;
        Consumer<String, KafkaMessage> consumer = getConsumer();
        ConsumerRecords<String, KafkaMessage> records = KafkaTestUtils.getRecords(consumer, 2000, 1);
        if (records.count() > 0) {
            record = records.iterator().next();
        }
        consumer.close();
        if (!isNull(record)) {
            if (!isNull(record.value())) {
                return record.value();
            }
        }
        return null;
    }

    private Consumer<String, KafkaMessage> getConsumer() {
        Map<String, Object> props =  new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaServer);
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "group_id");
        props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        props.put(JsonDeserializer.TRUSTED_PACKAGES, "*");

        ConsumerFactory<String, KafkaMessage> cf =  new DefaultKafkaConsumerFactory<>(
                props,
                new StringDeserializer(),
                new ErrorHandlingDeserializer<>(new JsonDeserializer<>(KafkaMessage.class, false)));


        Consumer<String, KafkaMessage> consumerServiceTest = cf.createConsumer();
        consumerServiceTest.subscribe(Collections.singleton(kafkaPrefix + "topic_name"));

        return consumerServiceTest;
    } {% endhighlight %}