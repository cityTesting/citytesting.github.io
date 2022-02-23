---
layout: post
title: Kafka integration testing (embedded Kafka)
categories: [java, kafka]
---

- Simulation of a consumer for testing our producer.

![](https://i.imgur.com/frha3N1.png?raw=true)  
{% highlight java %} import something 

@ExtendWith(SpringExtension.class)
@SpringBootTest
@DirtiesContext
@EmbeddedKafka(topics = { "TOPIC_OUT"})
public class ProducerServiceIntegrationTest {
    private static final String TOPIC_OUT = "TOPIC_OUT";

    @Autowired
    private EmbeddedKafkaBroker embeddedKafkaBroker;

    @Autowired
    private ProducerService producerService;

    public ExampleDTO mockExampleDTO(String name, String description) {
        ExampleDTO exampleDTO = new ExampleDTO();
        exampleDTO.setDescription(description);
        exampleDTO.setName(name);
        return exampleDTO;
    }

    /**
     * We verify the output in the topic. With an simulated consumer.
     */
    @Test
    public void itShould_ProduceCorrectExampleDTO_to_TOPIC_OUT() {
        // GIVEN
        ExampleDTO exampleDTO = mockExampleDTO("name", "description");
        // simulation consumer
        Map<String, Object> consumerProps = KafkaTestUtils.consumerProps("group_consumer_test", "false", embeddedKafkaBroker);
        consumerProps.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

        Consumer<String, ExampleDTO> consumerServiceTest = new KafkaConsumer<String, ExampleDTO>(
                consumerProps, new StringDeserializer(),
                new JsonDeserializer<>(ExampleDTO.class,
                        false));

        embeddedKafkaBroker.consumeFromAnEmbeddedTopic(consumerServiceTest, TOPIC_OUT);
        // WHEN
        producerService.send(exampleDTO);
        // THEN
        ConsumerRecord<String, ExampleDTO> consumerRecordOfExampleDTO = KafkaTestUtils.getSingleRecord(consumerServiceTest, TOPIC_OUT);
        ExampleDTO valueReceived = consumerRecordOfExampleDTO.value();

        assertEquals("description", valueReceived.getDescription());
        assertEquals("name", valueReceived.getName());

        consumerServiceTest.close();
    }
}  {% endhighlight %}

  
- Simulation of a producer for testing our consumer.

![](https://i.imgur.com/62RDZ3W.png?raw=true)


Java code:

<% highlight java %> 
//ConsumerServiceIntegrationTest
@ExtendWith(SpringExtension.class)
@SpringBootTest
@DirtiesContext
@EmbeddedKafka(topics = {"TOPIC_IN"})
public class ConsumerServiceIntegrationTest {
    Logger log = LoggerFactory.getLogger(ConsumerServiceIntegrationTest.class);

    private static final String TOPIC_IN = "TOPIC_IN";

    @Autowired
    private EmbeddedKafkaBroker embeddedKafkaBroker;

    @Autowired
    private ExampleRepository exampleRepository;

    public ExampleDTO mockExampleDTO(String name, String description) {
        ExampleDTO exampleDTO = new ExampleDTO();
        exampleDTO.setDescription(description);
        exampleDTO.setName(name);
        return exampleDTO;
    }

    /**
     * We verify the output in the topic. But aslo in the database.
     */
    @Test
    public void itShould_ConsumeCorrectExampleDTO_from_TOPIC_IN_and_should_saveCorrectExampleEntity() throws ExecutionException, InterruptedException {
        // GIVEN
        ExampleDTO exampleDTO = mockExampleDTO("Un nom 2", "Une description 2");
        // simulation producer
        Map<String, Object> producerProps = KafkaTestUtils.producerProps(embeddedKafkaBroker.getBrokersAsString());
        log.info("props {}", producerProps);
        Producer<String, ExampleDTO> producerTest = new KafkaProducer(producerProps, new StringSerializer(), new JsonSerializer<ExampleDTO>());
        // WHEN
        producerTest.send(new ProducerRecord(TOPIC_IN, "", exampleDTO));
        // THEN
        // we must have 1 entity inserted
        // We cannot predict when the insertion into the database will occur. So we wait until the value is present. Thank to Awaitility.
        await().atMost(Durations.TEN_SECONDS).untilAsserted(() -> {
            var exampleEntityList = exampleRepository.findAll();
            assertEquals(1, exampleEntityList.size());
            ExampleEntity firstEntity = exampleEntityList.get(0);
            assertEquals(exampleDTO.getDescription(), firstEntity.getDescription());
            assertEquals(exampleDTO.getName(), firstEntity.getName());
        });
        producerTest.close();
    }<% endhighlight %>  

All the infomation comes from this [blog](https://gitbook.deddy.me/test-dintegration-avec-spring-boot-et-kafka/).  
And all the code is inside [this](https://github.com/Kevded/integration-test-spring-kafka-with-embedded-kafka-consumer-and-producer) github repository..

  
