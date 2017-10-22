# Embeddable data services library using docker and testcontainers

If you are writing services using spring boot (and maybe spring cloud) and you do integration testing during build process then this set of autoconfigurations is for you.
By adding module into classpath you will get stateful service like mariadb or kafka auto-started and available for connection from your application service w/o wiring any additional code.

## How to use
### Make sure you have spring boot and spring cloud in classpath
```xml
<project>
...
      <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>spring-boot-starter-logging</artifactId>
                    <groupId>org.springframework.boot</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-log4j2</artifactId>
        </dependency>
  
...
</project>
```
### If you do not use spring cloud - make it work for tests
```xml
<project>
...
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            ...
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter</artifactId>
            ...
            <scope>test</scope>
        </dependency>
...
</project>
```
### Add data service library
```xml
<project>
...
        <dependency>
            <groupId>com.playtika.tests</groupId>
            <artifactId>testcontainer-kafka</artifactId>
            <scope>test</scope>
        </dependency>
...
</project>
```
### Use produced properties in your configuration
### Example:
/src/test/resources/application.properties

```properties
spring.kafka.zookeperHost=${embedded.kafka.zookeeperConnect}
spring.kafka.brokerList=${embedded.kafka.brokerList}
```
 /src/test/resources/bootstrap.properties
```properties
embedded.kafka.topicsToCreate=some_topic
```

## List of properties per data service
### embedded-kafka
#### Consumes
* embedded.kafka.enabled
* embedded.zookeeper.enabled
* embedded.kafka.topicsToCreate
* embedded.kafka.dockerImage (default is set to kafka 0.11.x)
### Produces
* embedded.kafka.zookeeperConnect
* embedded.zookeeper.zookeeperConnect
* embedded.kafka.brokerList

