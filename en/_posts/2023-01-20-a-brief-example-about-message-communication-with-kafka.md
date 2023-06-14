---
title: "A brief example about message communication with Kafka"
excerpt: >-
  This is a very entry level article about how to use Kafka for asynchronous communication model and message systems. It has
  just a simple example about how to use it.
comments: true
lang: "en-US"
permalink: /en/sample-about-communication-with-kafka
categories:
  - Software Engineering
  - Messaging Systems
  - Software Design
tags: 
  - software engineering
  - kafka
  - messaging
  - software design
---

{% include figure image_path="assets/images/sample-about-communication-with-kafka/Asynchronous-communication.png" alt="Asynchronous-communication.png" %}

This article came up as an idea when I was reviewing some content I already had studied, and I was working with. Since I needed to study a few more issues related 
to Kafka usage into application solutions I got the idea to share a few information related to that. It is a very entry level article. I'm not a Kafka specialist, 
but I already worked with solutions that needed to use asynchronous communication model and message systems. Based on this first information I'll try to approach 
the subject covering the following items:

- Bring some considerations when using synchronous and asynchronous scenarios in solutions;
- Sample of Kafka usage in the context of an asynchronous communication scenario (producer and consumer basics implementation).

The idea is **not** to explore the trade-offs over Kafka as technology or platform. So, there will be no deep dive through this direction. On this 
[AWS link](https://aws.amazon.com/pt/msk/what-is-kafka/) about Amazon Managed Streaming for Apache Kafka there are a few comparisons when looking to Kafka 
and RabbitMQ. This can give an idea about the differences when Kafka is compared with tools for messaging solutions.

## Synchronous and asynchronous communication

Over those years I have worked with IT it is possible to see that it is kind common see situations where developers have some difficulty distinguish when to use 
synchronous and asynchronous communication over solution scenarios. Most of the time it is possible to perceive some considerations missing in the evaluation of the 
requirements to design the solution to be implemented, or a mistaken evaluation over which could be a proper approach to be applied to some problem. It is very 
helpful the understanding of the main characteristics of both approaches when doing this evaluation.

### Synchronous vs. Asynchronous integration

In a summary, the main point that distinguish the synchronous communication from an asynchronous communication will be the need of receiving a response or not to 
keep processing something. When the response is needed for keep processing something, the synchronous communication scenario will be most of the time a better 
approach. When in a communication scenario it is possible or necessary to detach the producer behavior from the consumer behavior in such a way that a response 
is not needed to keep processing something, almost most of the time the asynchronous approach will fits better.

{% include figure image_path="assets/images/sample-about-communication-with-kafka/synch_vs_asynch.png" alt="synch_vs_asynch.png" %}

It is good to enforce that there is no silver bullet when we discuss solution design. The choices come with trade-offs to be analysed where the chosen option has 
good points and problems to be handled, and also there might happen some requirements that can invalidate some solution options. For example, it is possible 
to work with APIs for an asynchronous communication design if your corporation works only with API driven approach. The service gives a response to the request 
with a HTTP status code 202 to mention that something will be processed later. And them some callback endpoint can be called later when the processing of this 
request finishes. But this approach has its [trade-offs](https://aws.amazon.com/pt/blogs/architecture/managing-asynchronous-workflows-with-a-rest-api/).

When considering Kafka as a component in your solution it is possible to say that most of the cases its usage will fit into an 
[Enterprise Integration Pattern related to Messaging Patterns](https://www.enterpriseintegrationpatterns.com/patterns/messaging/index.html). With Kafka it combines 
two messaging models (queuing and publish-subscribe) to provide the key benefits of each to consumers. More details about this can be found on this 
[AWS link](https://aws.amazon.com/pt/msk/what-is-kafka/) about Amazon Managed Streaming for Apache Kafka.

## Producer and Consumer example using Kafka

Apache Kafka is a real-time data streaming technology capable of handling trillions of events per day. It is commonly used to build real-time streaming data 
pipelines and event-driven applications. It provides distributed, high-throughput, low-latency, fault-tolerant platform for handling real-time data feeds - 
known as events. More details can be found in the [Apache Kafka official page](https://kafka.apache.org/documentation/). The main idea here is to show a 
sample of Kafka usage to send and consume messages for asynchronous communication.

To illustrate a producer and an consumer it was created 2 Kotlin applications using Kafka as samples that consumes Tweets based on some parameters and save 
them in a ElasticSearh repository to allow search of the Tweets content. In a real world application it can be used to track subjects or themes that are 
trend topics for any specific reason. More details about the implementation itself can be checked [here](https://github.com/tnfigueiredo/kafka-twitter-dempo-app).

In this sample it was not deeply explored the set of [consumer configurations](https://kafka.apache.org/documentation/#consumerconfigs) or 
[producer configurations](https://kafka.apache.org/documentation/#producerconfigs). The main idea is to highlight a simple implementation to produce messages/events 
to Kafka and consume them. The source of the information are the filtered Twitters. All the components were linked to a docker-compose structure to make it easier 
to handle the sample.

![cached image](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/tnfigueiredo/kafka-twitter-dempo-app/main/kafka-twitter-producer-consumer-demo.puml)

In the applications it was created configuration components for having all the Kafka configuration for consumer and producer.

- [Consumer configuration](https://github.com/tnfigueiredo/kafka-twitter-dempo-app/blob/main/kafka-twitter-consumer-app/src/main/kotlin/com/example/kafkatweeter/consumer/app/config/KafkaConfigurator.kt):

```kotlin
   @Bean
    fun consumerFactory(): ConsumerFactory<String?, Any?> {
        val props: MutableMap<String, Any> = HashMap()
        props[ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG] = servers
        props[ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG] = StringDeserializer::class.java
        props[ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG] = StringDeserializer::class.java
        props[ConsumerConfig.AUTO_OFFSET_RESET_CONFIG] = "earliest"
        return DefaultKafkaConsumerFactory(props)
    }

    @Bean
    fun kafkaListenerContainerFactory(): ConcurrentKafkaListenerContainerFactory<String, Any>? {
        val factory = ConcurrentKafkaListenerContainerFactory<String, Any>()
        factory.consumerFactory = consumerFactory()
        factory.containerProperties.ackMode = ContainerProperties.AckMode.MANUAL_IMMEDIATE
        factory.containerProperties.isSyncCommits = true;
        return factory
    }
```

- [Producer configuration](https://github.com/tnfigueiredo/kafka-twitter-dempo-app/blob/main/kafka-twitter-producer-app/src/main/kotlin/com/tnfigueiredo/kafkatweeter/producer/app/config/KafkaConfigurator.kt):

```kotlin
    @Bean
    fun producerFactory(): ProducerFactory<String, Any> {
        val configProps: MutableMap<String, Any> = HashMap()
        configProps[ProducerConfig.BOOTSTRAP_SERVERS_CONFIG] = servers
        configProps[ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG] = true
        configProps[ProducerConfig.ACKS_CONFIG] = "all"
        configProps[ProducerConfig.COMPRESSION_TYPE_CONFIG] = "snappy"
        configProps[ProducerConfig.LINGER_MS_CONFIG] = 5
        configProps[ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG] = StringSerializer::class.java
        configProps[ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG] = StringSerializer::class.java
        return DefaultKafkaProducerFactory(configProps)
    }

    @Bean
    fun kafkaTemplate(): KafkaTemplate<String, Any> {
        return KafkaTemplate(producerFactory())
    }
```

Since the tweet message is a big JSon object I didn't try to map it to any object. It was serialized as a JSon String on both sides of the communication. The details 
about the tweets consumption is in the mentioned [GitHub repository](https://github.com/tnfigueiredo/kafka-twitter-dempo-app). After reading it, it is published to 
the Kafka topic:

```kotlin
@Component
class KafkaTweetsProducer(
    private val kafkaTemplate: KafkaTemplate<String, Any>
) {

    companion object {
        private val LOGGER = LoggerFactory.getLogger(KafkaTweetsProducer::class.java)
    }

    fun send(key: String, tweet: String) {
        LOGGER.info("Tweet message: {}", tweet)
        kafkaTemplate.send("tweets-message-topic", key, tweet)
    }

}
```

The consumer reads the tweet in the same way as it arrives from the Kafka topic to save it to the ElasticSearch repository:

```kotlin
@Component
class KafkaTweetsConsumer(
    private val elasticSearchClient: RestHighLevelClient,
    @Value("\${elasticsearch.index}")
    private val tweeterIndex: String
) {

    companion object {
        private val LOGGER = LoggerFactory.getLogger(KafkaTweetsConsumer::class.java)
    }

    @KafkaListener(topics = ["tweets-message-topic"], groupId = "simple-kotlin-tweets-message-consumer")
    fun consume(tweet: String) {
        LOGGER.info("got tweet: {}", tweet)
        val indexRequest = IndexRequest(tweeterIndex).source(tweet, XContentType.JSON)
        val indexResponse = elasticSearchClient.index(indexRequest, RequestOptions.DEFAULT)
        LOGGER.info("ElasticSearch id: {}", indexResponse.id)
    }
}
```

The README file from the repository has instructions to configure and run the sample. The result of the communication about the producer and the consumer can be 
seen in a Kibana interface:

![cached image](https://raw.githubusercontent.com/tnfigueiredo/kafka-twitter-dempo-app/main/kibana-criteria-search.png)
<br/>

## Final considerations

Kafka as technology and tool has a lot of real powerful use-cases. It is a technology worthy to explore for microservices approach, event driven architecture, 
asynchronous communication, real time processing, streaming data, and several other scenarios. I hope this article can be useful for an entry level reading about it.

## References

##### Asynchronous Communication; The What, The Why, And The How
* [Sandeep Kashyap: Asynchronous Communication; The What, The Why, And The How](https://www.proofhub.com/articles/asynchronous-communication)

##### Apache Kafka - Fundamentals, Use cases and Trade-Offs
* [Dhruvesh Patel: https://dev.to/dhruvesh_patel/apache-kafka-fundamentals-use-cases-and-trade-offs-9e4](https://dev.to/dhruvesh_patel/apache-kafka-fundamentals-use-cases-and-trade-offs-9e4)

##### Apache Kafka Documentation
* [Apache Kafka: https://kafka.apache.org/documentation/](https://kafka.apache.org/documentation/)

##### What is Kafka?
* [Confluent: https://www.confluent.io/what-is-apache-kafka/](https://www.confluent.io/what-is-apache-kafka/)

##### What is Kafka?
* [AWS: https://aws.amazon.com/pt/msk/what-is-kafka/](https://aws.amazon.com/pt/msk/what-is-kafka/)

##### Managing Asynchronous Workflows with a REST API
* [AWS: https://aws.amazon.com/pt/blogs/architecture/managing-asynchronous-workflows-with-a-rest-api/](https://aws.amazon.com/pt/blogs/architecture/managing-asynchronous-workflows-with-a-rest-api/)
