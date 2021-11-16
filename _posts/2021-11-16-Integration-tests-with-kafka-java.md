---
layout: post
title: Kafka integration testing 
categories: [Java, Kafka]
---
All the infomation comes from [here](https://gitbook.deddy.me/test-dintegration-avec-spring-boot-et-kafka/).

This method will use embedded Kafka.

- Simulation of a comnsumer for testing our Producer.

  ```
@ExtendWith(SpringExtension.class)
@SpringBootTest
@DirtiesContext
@EmbeddedKafka(topics = {TOPIC_OUT"})
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
        ExampleDTO exampleDTO = mockExampleDTO("Un nom", "Une description");
        // simulation consumer
        Map<String, Object> consumerProps = KafkaTestUtils.consumerProps(""group"", "false", embeddedKafkaBroker);
        consumerProps.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        ConsumerFactory cf = new DefaultKafkaConsumerFactory<String, ExampleDTO>(consumerProps, new StringDeserializer(), new JsonDeserializer<>(ExampleDTO.class, false));
        Consumer<String, ExampleDTO> consumerServiceTest = cf.createConsumer();
        embeddedKafkaBroker.consumeFromAnEmbeddedTopic(consumerServiceTest, TOPIC_OUT);

        // WHEN
        producerService.send(exampleDTO);

        // THEN
        ConsumerRecord<String, ExampleDTO> consumerRecordOfExampleDTO = KafkaTestUtils.getSingleRecord(consumerServiceTest, TOPIC_OUT);
        ExampleDTO valueReceived = consumerRecordOfExampleDTO.value();

        assertEquals("Une description", valueReceived.getDescription());
        assertEquals("Un nom", valueReceived.getName());

        consumerServiceTest.close();
    }
  ```
