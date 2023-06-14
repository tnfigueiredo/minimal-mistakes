---
title: "Web APIs - Different approaches and how to choose"
excerpt: >-
  This article brings a vision related to the possible different APIs approaches and the possible scenarios where those approaches 
  can be used to solve the proposed problem.
comments: true
lang: "en-US"
permalink: /en/web-apis-approaches
categories:
  - Software Engineering
  - APIs
  - Software Design
tags: 
  - architecture
  - software design
  - API
---

![api-top-image]({{ site.url }}{{ site.baseurl }}/assets/images/web-apis-approaches/apis-top.png){: .align-center}

Web APIs are one of the most common resources used for design several solutions related to integration scenarios, microservice 
applications, and other very common solutions that are built on top of the HTTP protocol.

![api-big-pic]({{ site.url }}{{ site.baseurl }}/assets/images/web-apis-approaches/apis-big-pic.png){: .align-center}

If we look to WEB APIs as a solution alternative, it is possible to see that it became an interesting option because it works 
over an interoperable protocol widely used. This option mitigates several problems that we might face when building the system's 
integration and features. Since we are talking about APIs, it is important to understand some principles that we need to use 
to guide a solution approach: APIs – Application program interface – are most commonly expressed as a set of operations, 
associated data definitions, and the semantics of the operations on some underlying system (David Emery - Association for 
Computing Machinery).

There are other similar definitions, but taking this one as a basis we can see that semantic is something very important when 
designing an API. The API semantics depends on the WEB API Design approach, its usage of HTTP verbs, definitions about application 
operations, and other elements. And in several moments development teams will question themselves which approach they need to use. 
Those choices come to the trade-off analysis. The best tool to guide this analysis is to understand better the WEB APIs Design 
approaches and check which one fits better to your problem requirements.

## Web API Design approaches

When evaluating possible Web API design approaches it will fit into a few possible options: RPC, REST, or a "query language" API 
style (GraphQL). Basically, those are concepts of how to design your API that follow specifications that are often drafted up by 
various working groups. For example: SOAP is a W3C recommendation, gRPC has as authors Google Inc., REST is a paradigm with no 
organization or group responsible for it.

By checking on the characteristics of each design style it will be possible to start evaluating which approach matches your problem. 
When looking at the RPC style, it will depend on the specific API implementation. REST and GraphQL have more parameters related to 
the standards and paradigms.

### RPC

RPC APIs are about executing a block of code on another server, that when implemented in HTTP or AMQP can become a Web API. It is 
possible to say that:
* They are action-based;
* The API specification depends on the implementation;
* Can be stateless or stateful;
* No specific semantic definition:
  - No HTTP verbs best practice recommendation;
  - No HTTP status code usage recommendation;
  - Best practices recommendations depend on the chosen RPC API.

The development of an RPC API is similar to create programming libraries. The name of the actions are in the URI, and they can be 
compared to a function invocation. Definitions of parameters and returns according to the operation needs in the query string or body, 
7and modern implementations can make simple interactions with high performance. There is no discoverability (How to start? What to call?). 
In general, can become hard to understand and maintain due to the tendency to create new endpoints when new functions are needed. 
This creates a tight coupling solution.

### REST

REST is a paradigm described by Roy Fielding in a dissertation in 2000. It describes a client-server relationship where server-side data are 
made available through representations of data in simple formats. To be considered a RESTful API it is necessary to fit into the restrictions 
described in this dissertation. It uses as most common formats JSON or XML but could be anything. When describing a RESTful API we can say that:
* It is resource-based;
* Nouns of the resources are used to define URIs;
* It is stateless;
* There is a strict semantic definition based on HTTP:
  - It has HTTP verbs and status code semantic usage;
  - Resources and their relationships represented in the API’s URIs;
  - Content negotiation for different message formats;
* Service URIs are easily readable due to their strict semantic;
* Hypermedia allows actions and relationships to be made discoverable.

![api-scope]({{ site.url }}{{ site.baseurl }}/assets/images/web-apis-approaches/apis-scope.jpg){: .align-center}

The RESTful APIs are considered good options when there are issues involved like common workflows to be followed, caching of resource 
information, not a very reduced payload restriction, content negotiation necessary to provide a response to the clients, etc.

## GraphQL and "Query language" APIs

GraphQL was built by Facebook and it is a data-driven API design approach that provides a query language for APIs and a runtime for 
fulfilling those queries with your existing data. GraphQL can be essentially considered an RPC API style with a default procedure, 
with a lot of good ideas from the REST/HTTP community. It is possible to mention that its main characteristics are:

* Stateless;
* It is based on entity graphs and entities are not identified by URIs;
* Custom data recovery:
  - Ask for specific resources and specific fields;
  - Operation parameters and returns parametrized;
* Reduced number of HTTP requests necessary to retrieve data for multiple resources;
* Low network overhead.
  <br/>
  <br/>

# How to choose?

It is important to look into the requirements that the application needs to meet, so it becomes possible to look at the different 
API design approaches to find which one fits best into the problem to be solved. Based on the highlighted issues of each API style 
that were mentioned it is possible to illustrate some scenarios.

![thinking-api]({{ site.url }}{{ site.baseurl }}/assets/images/web-apis-approaches/thinking-api.png){: .align-center}

Among the different approaches that were presented, when evaluating the options through RPC APIs it will depend on the specific 
implementation option. For example, gRPC can be used in scenarios where a system requires a set amount of data or processing routinely, 
and in which the requester is either low power or resource-jealous (IoT is a great example of this). Or scenarios like SOAP 
interaction where is needed asynchronous processing and invocation, formal contracts, and stateful operations.

One good scenario to explain where REST fits is when we bring the reality of microservice applications, where different bounded 
contexts must be decoupled. Things within a context can treat their own APIs in private and it is possible to do changes whenever 
needed without affect other domains. It is even possible to use an RPC approach when pieces inside a bound context need to 
communicate internally. But when there is communication among different contexts it is important to be loosely coupled. Another 
scenario is when this interaction between client and server requires a workflow among a set of well-defined resources (for example, 
a payment workflow).

![contexts-interaction]({{ site.url }}{{ site.baseurl }}/assets/images/web-apis-approaches/contexts-interaction.jpeg){: .align-center}

Proposing a scenario where GraphQL would be a good fit, we can consider an interaction among the client of an API where to recover 
information needed will demand several requests or iterations to achieve the final purpose. For example, lest suppose there is the 
need to call “/users/” endpoint to fetch the initial user data, then call a “/users//posts” endpoint to return all the posts for a user, 
and then call a “/users//followers” to return a list of followers per user.

![sample-api-requests]({{ site.url }}{{ site.baseurl }}/assets/images/web-apis-approaches/sample-api-requests.png){: .align-center}

When evaluating this interaction through the perspective of a data-driven API this information could be recovered into a single interaction. 
It is possible to be considered also in a scenario where the relationship between the resources is hard to be handled through a set of 
interactions into RESTful resources. Or also when there is needed to recover a customized set of information involving fields of different 
resources to compose a single response.

![contexts-interaction-2]({{ site.url }}{{ site.baseurl }}/assets/images/web-apis-approaches/sample-api-requests-2.png){: .align-center}

## References

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
