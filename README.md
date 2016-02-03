# Docker-compose project for ELK stack
ELK stack consists of three applications (Elasticsearch, Kibana and Logstash) that enables centralized logging with potenially powerful analytical capabilities. 
In this project we use the official Docker images of these three applications to enable remote logging from an application.

Elasticsearch is the database where logs are stored. Kibana is a tool to analyze the logs stored in Elasticsearch. Logstash is used to route and filter incomming logs and output them into (for instance) Elasticsearch.

To be able to log to Elasticsearch the logs need to be in JSON format.

## JSON logging from a Java application
To be able to log to the ELK stack from a Java application you should use the Logstash Logback Encoder: https://github.com/logstash/logstash-logback-encoder

### Using the Logstash TCP appender
This approach is only viable on a Java application that uses logback.
Following is the simplest example of configuring this appender (logback.xml configuration (note this is insecure)):

---
```xml
<appender name="stash" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
    <destination>my-logstash-host:8300</destination>

    <!-- encoder is required -->
    <encoder class="net.logstash.logback.encoder.LogstashEncoder" />
</appender>

<!-- ... -->

<root level="INFO">
    <!-- other appenders you might have here -->
    <appender-ref ref="stash"/>
</root>
```
---

## Kibana
Kibana is where you can analyze your logs. To access Kibana just open a browser at the server where Kibana is running on port 5601.
