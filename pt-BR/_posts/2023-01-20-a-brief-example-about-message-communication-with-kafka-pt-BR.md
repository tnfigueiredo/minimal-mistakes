---
title: "Um breve exemplo sobre comunicação via mensagem com Kafka"
excerpt: >-
  Esse é um artigo de nível bem iniciante sobre como usar Kafka para modelo de comunicação assíncrona e sistemas de mensagem. Ele tem
  apenas um exemplo simples de como utilizar essa ferramenta.
comments: true
lang: "pt-BR"
permalink: /pt-BR/sample-about-communication-with-kafka
toc: true
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

Esse artigo surgiu como uma ideia quando eu estava revisando conteúdos que eu já havia estudado, e com os quais já trabalhei. Como precisava estudar alguns outros 
tópicos relacionados ao uso de Kafka em soluções de aplicação tive a ideia de compartilhar um pouco de informação. É um artigo de nível bem iniciante. Não sou um 
especialista em Kafka, mas já trabalhei com soluções que utilizavam o modelo de comunicação assíncrona e sistemas de mensageria. Baseado nisso, tentarei explorar 
o assunto cobrindo os seguintes items:

- Trazer algumas considerações sobre cenários de solução de comunicação síncrona e assíncrona;
- Exemplo de uso do Kafka no contexto de um cenário de comunicação assíncrona (implementação básica de producer e consumer).

A ideia **não** é explorar os trade-offs sobre Kafka como tecnologia ou plataforma. Então, não haverá aprofundamento nessa direção. Sobre o tema o  
[AWS link](https://aws.amazon.com/pt/msk/what-is-kafka/) relacionado com o serviço Amazon Managed Streaming para Apache Kafka tem algumas comparações 
quando compara Kafka e RabbitMQ. É possível ter uma ideia sobre as diferenças quando Kafka é comparado com ferramentas de solução de mensageria.

## Comunicação síncrona e assíncrona

Nesses anos em que tenho trabalho com TI é possível ver como uma situação comum cenários onde desenvolvedores tem dificuldade de distinguir sobre quando 
usar comunicação síncrona e assíncrona em cenários de solução. Muitas vezes é possível perceber a falta da avaliação de alguns detalhes dos requisitos 
para projetar a solução a ser implementada, ou uma avaliação errada sobre quais poderiam ser a abordagem apropriada a ser aplicada a um problema. É de grande 
ajuda o entendimento das principais características de ambas as abordagens quando se está fazendo essa avaliação.

### Integração Síncrona vs. Assíncrona

Em resumo, o principal ponto de destingue a necessidade de comunicação síncrona e assíncrona será a necessidade de precisar ou não receber uma resposta 
de uma requisição para poder continuar o processamento de alguma outra coisa. Quando a resposta é necessária para processar algo, o cenário de comunicação síncrona 
será geralmente a melhor abordagem. Quando num cenário de comunicação for possível ou necessário desacoplar o comportamento do produtor da informação 
do comportamento do consumidor de forma que a resposta não seja necessária para continuar o processamento de algo, geralmente a abordagem assíncrona será melhor.

{% include figure image_path="assets/images/sample-about-communication-with-kafka/synch_vs_asynch.png" alt="synch_vs_asynch.png" %}

Importante reforçar que não existe bala de prata quando se discute projeto de solução. As escolhas vêm com trade-offs a serem analisados onde a escolha tem
pontos positivos e problema a serem tratados, assim como podem existir alguns requisitos que podem invalidar algum candidato à solução. Por exemplo, É possível 
trabalhar com APIs para um projeto de comunicação assíncrona se a corporação trabalha com uma abordagem API driven. O serviço devolve uma resposta para a requisição 
com um HTTP status code 202 para informar que algo vai ser processado no futuro. E algum endpoint de callback do cliente pode ser chamado posteriormente quando esse 
processamento terminar. Mas essa abordagem tem esses [trade-offs](https://aws.amazon.com/pt/blogs/architecture/managing-asynchronous-workflows-with-a-rest-api/).

Ao considerar Kafka como um componente em sua solução é possível dizer que a maioria dos casos ele vai se encaixar em um  
[Enterprise Integration Pattern relacionado à Messaging Patterns](https://www.enterpriseintegrationpatterns.com/patterns/messaging/index.html). Com Kafka é possível  
combinar dois modelos de mensageria (filas e publish-subscribe) para prover os benefícios chave para cada consumidor. Mais detalhes podem ser encontrados nesse 
[AWS link](https://aws.amazon.com/pt/msk/what-is-kafka/) sobre Amazon Managed Streaming para Apache Kafka.

## Exemplo de Producer e Consumer usando Kafka

> Apache Kafka é uma tecnologia de streaming de dados real-time capaz de lidar com trilhões de eventos por dia. É comumente utilizada para construir real-time streaming 
data pipelines e aplicações event-driven. Provê uma plataforma distribuída, com high-throughput, low-latency, e tolerância a falhas, para lidar com feeds de dados real-time - 
conhecidos como eventos. Mais detalhes podem ser encontrados em [Apache Kafka official page](https://kafka.apache.org/documentation/). 

A principal ideia é mostrar um exemplo com Kafka para envio e consumo de mensagens em uma comunicação assíncrona. Para ilustrar um producer e um consumer foram criadas 
2 aplicações Kotlin utilizando Kafka como exemplo em que Tweets são consumidos baseado em algum parâmetro e sendo o evento salvo em um repositório ElasticSearh 
que permita pesquisar o conteúdo dos Tweets. Em uma aplicação de real-time isso poderia ser usado para acompanhar em tempo real assuntos ou temas que são  
trend topics por qualquer motivo em específico. Mais detalhes sobre a implementação podem ser verificados [aqui](https://github.com/tnfigueiredo/kafka-twitter-dempo-app).

Nesse exemplo não foi profundamente explorado o conjunto de [configurações do consumer](https://kafka.apache.org/documentation/#consumerconfigs) ou 
[configurações do producer](https://kafka.apache.org/documentation/#producerconfigs). A ideia principal era ressaltar um exemplo de implementação para produzir mensagens/eventos 
para um Kafka e consumi-los. A fonte de informação são os Tweets filtrados. Todos os componentes estão relacionados em uma estrutura docker-compose para facilitar o uso 
e manusear o exemplo.

![cached image](http://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/tnfigueiredo/kafka-twitter-dempo-app/main/kafka-twitter-producer-consumer-demo.puml)

Nas aplicações foram criadas configurações de componentes para ter todas as configurações de Kafka para consumers e producers.

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

Uma vez que as mensagens de tweet são objetos JSon grandes não tentei mapeá-los para objetos específicos. Foram serializados como uma String JSon em ambos os lados da comunicação. Os detalhes 
sobre o consumo das mensagens de tweet estão no seguinte [GitHub repository](https://github.com/tnfigueiredo/kafka-twitter-dempo-app). Após lê-las, é possível publicá-las 
em um tópico Kafka:

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

O consumer lê o tweet do mesmo modo que ele chega do tópico Kafka para salvar no repositório ElasticSearch:

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

O arquivo README do repositório tem instruções para configurar e rodar o exemplo. O resultado da comunicação entre o producer e consumer pode ser 
visto em uma interface do Kibana:

![cached image](https://raw.githubusercontent.com/tnfigueiredo/kafka-twitter-dempo-app/main/kibana-criteria-search.png)

## Considerações finais

Kafka como ferramenta e tecnologia atende vários use-cases reais poderosos. É uma tecnologia válida de ser explorada para abordagem de microserviços, arquitetura event driven,  
de comunicação assíncrona, processamento real time, streaming data, e vários outros cenários. Espero que esse artigo possa ser útil para uma leitura de nível iniciante.

## Referencias

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
