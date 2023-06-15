---
title: "Web APIs - Diferentes abordagens e qual escolher"
excerpt: >-
  Esse artigo traz uma visão relacionada às possíveis diferentes abordagens de APIs e os possíveis cenários onde essas abordagens  
  podem ser utilizadas para determinados tipos de problema.
comments: true
lang: "pt-BR"
permalink: pt-BR/web-apis-approaches
toc: true
categories:
  - Software Engineering
  - APIs
  - Software Design
tags: 
  - architecture
  - software design
  - API
---

{% include figure image_path="/assets/images/web-apis-approaches/apis-top.png" alt="apis-top-image" %}

Web APIs são um dos recursos mais comumente utilizados para projetar diversas soluções relacionadas a cenários de integração, aplicações de 
microserviços, e outros cenários comuns de solução que são construídos sobre o protocolo HTTP.

![api-big-pic]({{ site.url }}{{ site.baseurl }}/assets/images/web-apis-approaches/apis-big-pic.png){: .align-center}

Se olharmos para as alternativas de solução de WEB APIs, é possível dizer que é uma opção interessante porque elas trabalham 
sobre um protocolo amplamente utilizado. Essa opção mitiga diversos problemas que podem surgir quando se desenvolve integração 
e funcionalidades de sistemas. Uma vez que o tema é APIs, é importante entender alguns princípios necessários para  
guiar as abordagens de solução: 

> APIs – Application program interface – são comumente definidas como um conjunto de operações, associadas a definições de dados, e a semântica 
de operações em algum sistema subjacente (David Emery - Association for Computing Machinery).

Existem outras definições similares, mas tomando essa cmo base é possível dizer que a é algo muito importante quando está sendo  
projetada uma API. A semântica da API depende da abordagem de projeto de WEB API, sua utilização dos verbos HTTP, definições sobre 
operações da aplicação, e outros elementos. Em diversos momentos times de desenvolvimento vão se questionar sobre qual abordagem eles 
precisam usar. Essas escolhas precisam ser embasadas em uma análise de trade-offs. A melhor ferramenta para guiar essa análise é entender 
melhor as abordagens de projeto de WEB APIs e verificar quais se encaixam melhor para os requisitos do seu problema.

## Abordagens de projetos de Web API

Quando avaliamos as possíveis abordagens de projetos de Web API será possível observar as seguintes opções: RPC, REST, ou um estilo "query language" 
de API (GraphQL). Basicamente, esses são conceitos sobre como projetar API que seguem especificações frequentemente elaboradas por 
vários grupos de trabalho. Por exemplo: SOAP é uma recomendação da W3C, gRPC tem como autores Google Inc., REST é um paradigma sem organização ou grupo 
responsável pela especificação.

Ao checar as características de cada estilo de projeto de API é possível iniciar uma avaliação de quais estilos estão relacionados com o problema 
da sua solução. Ao avaliar o estilo RPC, será necessário verificar a implementação em específico. REST e GraphQL tem mais caracterísicas relacionadas 
a padrões e paradigmas.

### RPC

APIs RPC funcionam fundamentadas no princípio da execução de um bloco de código em outro servidor, que quando implementado através do HTTP ou AMQP pode ser 
considerada uma Web API. É possível dizer que:

* Elas são baseadas em ações/funcionalidades;
* A especificação da API specification depende da implementação;
* Podem ser com ou sem estado;
* Não tem uma semântica específica definida:
  - Não há recomendação de melhores práticas sobre o uso dos verbos HTTP;
  - Não há recomendação de dos HTTP status code;
  - Melhores práticas dependem da implementação de API RPC escolhida.

O desenvolvimento de uma API RPC é similar à criação de bibliotecas de programação. Geralmente o nome das ações fazem parte da URI, e elas podem ser comparadas
a funções que são chamadas. Definições dos parâmetros e retornos são conforme as necessidades da operação passadas via query string ou body, 
e implementações modernas podem fazer iterações simples e com alto desempenho. Não existe mecanismo de discoverability (Por onde começar? O que chamar?). 
Em geral, podem se tornar difíceis de ler e manter devido à tendência de criar endpoints quando novas funções são necessárias. Algo que geralmente cria 
soluções com alto acoplamento.

### REST

REST é um paradigma descrito por Roy Fielding em uma dissertação do ano 2000. Ela descreve um modelo cliente-servidor onde do lado do servidor  
os dados são disponibilizados em uma representação em formatos simples. Para ser considerada uma API RESTful é necessário estar enquadrada em algumas 
restrições descritas nessa dissertação. Os formatos de dados mais comuns são JSON ou XML, mas pode ser qualquer formato. Ao descrever uma API RESTful 
é possível dizer que:
* É baseada no conceito de recursos;
* Nomes dos recursos são utilizados para definir as URIs;
* Não há estado;
* Tem uma semântica bem definida e definições baseadas no HTTP:
  - Usa a semântica dos verbos e status code do HTTP;
  - Recursos e seus relacionamentos são representados nas URIs da API;
  - Negociação de conteúdo para diferentes formatos solicitados nas requisições;
* URIs dos serviços são facilmente legíveis devido sua semântica bem definida;
* Hypermedia permite que ações e relacionamentos possam tornar-se pesquisáveis.

{% include figure image_path="/assets/images/web-apis-approaches/apis-scope.jpg" alt="apis-scope" caption="APIs' styles scope" %}

As APIs RESTful são consideradas uma boa opção quando existem workflows comuns a serem seguidos, caching de informação dos recursos é necessário, 
quando não há restrição de payload muito reduzido, quando negociação de conteúdo é necessária para prover uma resposta aos clientes, etc.

## GraphQL e APIs de "Query language"

GraphQL foi criado pelo Facebook. É uma abordagem de projeto de API data-driven que provê uma query language para APIs e um ambiente de runtime 
para criação das queries com um modelo de dados existente. GraphQL pode ser essencialmente considerado um estilo de API RPC com um procedimento 
padrão, contendo ainda ótimas ideias da comunidade REST/HTTP. É possível mencionar que suas principais características são:

* Sem estado;
* É baseada em grafos de entidades e as entidades não são usadas para construir as URIs;
* Recuperação de dados customizável:
  - Possível solicitar recursos específicos e campos específicos;
  - Parâmetros da operação e retornos parametrizados;
* Reduz o número de requisições HTTP necessárias para retornar dados de múltiplos recursos;
* Baixa sobrecarga de rede.

# Como escolher?

É importante olhar para os requisitos que a aplicação precisa atender, permitindo olhar para as diferentes abordagens de projetos de  
API para ver qual se encaixa melhor no problema a ser resolvido. Baseado nos itens destacados de cada estilo de projeto de API que foram 
mencionados é possível ilustrar alguns cenários.

![thinking-api]({{ site.url }}{{ site.baseurl }}/assets/images/web-apis-approaches/thinking-api.png){: .align-center}

Entre as diferentes abordagens que foram apresentadas, ao avaliar as opções de APIs RPC haverá dependência conforme as opções de 
implementação. Por exemplo, gRPC pode ser usado onde o sistema precisa de um conjunto de dados ou processamento rotineiro, 
e no qual é necessário baixo consumo de energia ou carente de recursos (IoT é um bom exemplo para tal). Ou, por exemplo, cenários onde 
interação SOAP onde é necessário processamento assíncrono e chamadas com contratos formais, e operações com estado.

Um bom cenário para explicar onde REST é quando temos a realidade de aplicações de microserviços, onde diferentes contextos de negócio podem 
conviver desacoplados. Elementos pertencentes a um determinado contexto podem tratar suas operações numa mesma API em operações privadas, 
e é possível fazer modificações sempre que necessário sem afetar outros contextos. É possível ainda usar uma abordagem RPC pedaços num 
contexto de negócio precisam se comunicar internamente. Mas quando há comunicação entre diferentes contextos é importante manter um baixo acoplamento. 
Outro cenário é quando essa interação entre cliente e servidor requer um workflow entre um conjunto de recursos bem definidos 
(por exemplo, um workflow de pagamentos).

{% include figure image_path="/assets/images/web-apis-approaches/contexts-interaction.jpeg" alt="contexts-interaction.jpeg" %}

Propondo um cenário onde GraphQL poderia ser uma boa escolha, podemos considerar uma interação entre um cliente de uma API onde para recuperar uma informação 
necessária seria preciso várias chamadas para um servidor e iterações para alcançar o objetivo final. Por exemplo, vamos supor que é necessário fazer uma chamada 
para o endpoint do recurso “/users/” para carregar os dados iniciais do usuário, então chamar o endpoint “/users//posts” todos os posts de um usuário, 
e então chamar um endpoint “/users/followers” para retornar a lista de seguidores de um usuário.

{% include figure image_path="/assets/images/web-apis-approaches/sample-api-requests.png" alt="sample-api-requests.png" caption="RESTful API calls example" %}

Ao avaliar essa interação por uma perspectiva de data-driven API essa informação poderia ser recuperada toda em uma única interação. 
É possível considerar isso também em um cenário onde o relacionamento entre os recursos é difícil de ser gerenciado através de um conjunto de interações 
em uma API de recursos RESTful. Ou também quando existe a necessidade de recuperar dados customizados de um conjunto de informações envolvendo 
campos de diferentes recursos para compor uma única resposta.

{% include figure image_path="/assets/images/web-apis-approaches/sample-api-requests-2.png" alt="sample-api-requests-2.png" caption="GraphQL API calls example" %}

## Referencias

##### Standards, APIs, Interfaces and Bindings
* [David Emery - Association for Computing Machinery: http://oldwww.acm.org/tsc/apis.html](http://oldwww.acm.org/tsc/apis.html)

##### Understanding RPC, REST and GraphQL
* [Phil Sturgeon: https://apisyouwonthate.com/blog/understanding-rpc-rest-and-graphql](https://apisyouwonthate.com/blog/understanding-rpc-rest-and-graphql)

##### Picking the right API Paradigm
* [Phil Sturgeon: https://apisyouwonthate.com/blog/picking-the-right-api-paradigm](https://apisyouwonthate.com/blog/picking-the-right-api-paradigm)

##### GraphQL — The Query language for APIs
* [Arun Rajeevan: https://arunrajeevan.medium.com/graphql-the-query-language-for-apis-4e79ae303100](https://arunrajeevan.medium.com/graphql-the-query-language-for-apis-4e79ae303100)

##### When to Use What: REST, GraphQL, Webhooks, & gRPC
* [Kristopher Sandoval: https://nordicapis.com/when-to-use-what-rest-graphql-webhooks-grpc/](https://nordicapis.com/when-to-use-what-rest-graphql-webhooks-grpc/)

##### SOAP vs REST 101: Understand The Differences
* [SoapUI: https://www.soapui.org/learn/api/soap-vs-rest-api/](https://www.soapui.org/learn/api/soap-vs-rest-api/)
