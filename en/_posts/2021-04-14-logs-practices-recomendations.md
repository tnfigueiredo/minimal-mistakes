---
title: "Logs - Why, good practices, and recommendations"
excerpt: "A bunch of text to test readability."
comments: true
lang: "en-US"
permalink: /en/log-practices-recommendations
tags: 
  - good practices
  - software engineering
---

![logs-top-image]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/logs.png){: .align-center}

## Why logging is important?

Logging information is important to understand the behavior of the application and to understand problems in several different scenarios. Into the application lifetime, 
it will eventually crash, a server will go down, users may complain about a bug that “randomly” appears, or the client could realize that some data is missing, and there is no 
clue when or why this data got lost. Those are some examples of scenarios where there is a need to understand the application behavior through the development application 
lifecycle, track problems during QA validations and incident scenarios, and other possible situations. That is why the application solution logs must be designed, implemented, and tested.

Considering those issues related to the importance of logs it is necessary to define for the application what, how, and when to log and even how to extract meaningful 
data from logs. This brings the need to define a structure for log messages. The need to define a log structure is to extract the information needed (most of the 
time with the support of tools).

![log-structure]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/log-structure.jpeg){: .align-center}

Another issue that is worthy to mention is that log messages need to be sent at least in every layer of the architecture with meaningful and enough contextual information. 
Having at least one log entry per request/result in every layer of the application allows providing accurate context about what the user was doing when a specific error 
or situation happened.

Once it is defined the structure of the log messages, the next step is to have clear what to log and how to log. The log messages for the application need to inform 
about errors (to use them for troubleshooting purposes), but also useful information about successful requests to have a clear idea of how users work with the application. 
In this subject, we get through the usage of log level messages related to the purpose of the information we want to inform.

## Logging levels

The usage of a good logging strategy needs to take into consideration the proper usage of log levels for its messages. This strategy related to log level messages is essential 
to avoid “noise” when looking for information and track possible evidences to evaluate a problem or analyze the application behavior. The main cons about this are that when a 
problem comes up, you are likely to proper contextual information to be analyzed.

![loggin-levels]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/loggin-levels.png){: .align-center}

There will be frameworks to help handle logging messages for the different programming languages and the definition among the log levels can slightly vary among the 
framework implementations. But they follow the same semantic purpose. The following example shows this for a few Java frameworks possible to be used.

![logging-level-config]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/logging-level-config.png){: .align-center}

The log level information has well-defined usage recommendations for each situation:

![logging-level-behaviour]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/logging-level-behaviour.png){: .align-center}

## Good practice recommendations

According to the presented reasons over the application logging practices, there are a few good practices and recommendations to be followed to provide log messages that are meaningful and useful.

### General good practices

* Set the current log level via external configuration: the modification of the current log level messages should always be an operational task done in runtime. This allows fast action to problem troubleshooting and situation analysis without stopping the application.

* Default log level information configuration by environments: For the production environment it is interesting to have the default log level as “Information”. This will allow that Fatal, Error, Warning, and Information log entries will be written. while Debug and Trace log entries will be suppressed. Environments that need more detailed information can have default log level set to debug or trace.

* The proper log level is important to avoid noise: log application information with the proper log level avoids mistaken interpretation between data that represent problems and data that represents application behavior:

    - Use the Warning and Information Log Levels properly: Warning should be for logs that are not errors, but are not normal behavior. Information should be for application normal execution when something important is to be communicated.

    - Log catastrophic failures using Critical/Fatal: when there is an unrecoverable error during the application start-up or execution log it using the Critical log level. Tools can use this log level for emitting alerts.

    - Step through code using the Debug log level: When your application is misbehaving it is necessary to get enhanced visibility into what is going on. The good use of the Debug log level can get you more detailed about what is occurring.

    - Inspect variables using the Trace log level: The use of trace log level to write the values of variables, parameters, and settings to the logs for inspection can help you when the debug log level isn’t enough. This mimics the behavior of inspecting the contents of variables with the debugger attached.

* Never log personal identifying Information or secrets: It is important because of GDPR, CCPA, and other privacy laws and for security issues.

### Events log good practices

* Use clear key-value pairs: tolls extract fields from events when you search, creating structure out of unstructured data.

* Create events that humans can read: complex encoding that would require lookups to make event information intelligible.

* Use timestamps for every event: The correct time is critical to understanding the proper sequence of events, for debugging, analytics, and deriving transactions.

* Use correlation IDs for tracking operations: Correlation ID (also known as a Transit ID) is a unique identifier value that is attached to requests and messages that allow reference to a particular transaction or event chain. They are helpful when debugging and tracking transactions through the system and follow them across machines, networks, and services.

* Avoid logging binary information: It is not good for reading what is happening through the log events and also does not work well when tools are indexing the information.

* Use data structured formats: They are readable by humans and machines and can be easily parsed by most programming languages right in your browser. This helps tools and log search when evaluating log events.

* Identify the source (class, function, or filename): Useful to understand where the problem is.

### Operational good practices

* Log locally to files: If you log to a local file, it provides a local buffer and you aren't blocked if the network goes down.

* Use tools for streaming and monitoring logs: Tools can collect logging data and then send this information to the indexers. They will be able to work well among the large amount of information generated by the logging activity.

* Use rotation and retention policies: Logs can take up a lot of space. Good rotation strategies can help to decide when to destroy or back up your logs (if needed). It is especially essential considering scenarios for cloud solutions where storage price is important.

* Collect events from all possible sources: The more data you capture, the more visibility you have. Some example of possible sources:
    - Application logs;
    - Database logs;
    - Network logs;
    - Configuration files;
    - Performance data.

## Tools for dealing with log information

Without tools for dealing with log information, the amount of data created by logging activity can be meaningless. Besides that, in a scenario where there are several instances of distributed 
applications and several services that those applications integrate, those tools are helpful to track information that comes from different sources. Each of those sources formats and stores 
logs in their own way, making it really difficult to find useful data.

The logging tools that we have available do streaming over log messages and centralize its access, allowing log search and aggregation to turn all this generated data into useful information. 
There are several examples of logging tools nowadays, but here follow the example of the structure of two of them. The first image is from an example using splunk, and the following image 
is an example using the Elastic stack:

![example-stack-1]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/example-stack-1.png){: .align-center}

![example-stack-2]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/exmaple-stack-2.png){: .align-center}

Those tools get the log messages through collectors and store the information into repositories where the information is indexed. When the collectors get the information, they analyze the 
log structure to process its data before storage. Once the log messages are stored and indexed it is possible to search information, create charts and create alerts through the interface of 
those tools. Here follow a few examples of the mentioned log searching UI. The presented UIs are from the tools Splunk, Graylog, and Kibana.

Splunk:

![log-reader-example-1]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/log-reader-example-1.png){: .align-center}

![log-reader-example-2]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/log-reader-exmaple-2.jpeg){: .align-center}

![log-reader-example-3]({{ site.url }}{{ site.baseurl }}/assets/images/logs-practices-recommendations/log-reader-exmaple-3.jpeg){: .align-center}

As it is possible to see, those tools provide very interesting features related to logging activity. That brings a good reason to work with the messages following a well-defined structure, good practices, and recommendations. It makes the log information possible to be processed, tracked, and handled by the referred tools so they become useful.

## References

##### Logging Best Practices:
[Ray Saltrelli - https://dev.to/raysaltrelli/logging-best-practices-obo/](https://dev.to/raysaltrelli/logging-best-practices-obo)

##### Logging best practices in an app or add-on for Splunk Enterprise:
[https://dev.splunk.com/enterprise/docs/developapps/addsupport/logging/loggingbestpractices/](https://dev.splunk.com/enterprise/docs/developapps/addsupport/logging/loggingbestpractices/)

##### The Importance of logging: introducing Elastic Stack
[Christian Claudio Bohm - https://www.hexacta.com/importance-logging-introducing-elastic-stack/](https://www.hexacta.com/importance-logging-introducing-elastic-stack/)

##### 9 Logging Best Practices Based on Hands-on Experience
[Liron Tal - https://www.loomsystems.com/blog/single-post/2017/01/26/9-logging-best-practices-based-on-hands-on-experience](https://www.loomsystems.com/blog/single-post/2017/01/26/9-logging-best-practices-based-on-hands-on-experience)