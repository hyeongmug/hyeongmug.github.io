---
category: MSA
tags: [msa, springboot, springcloud]
---


## Spring Boot 2.4 이전

bootstrap.yml :

``` yml
spring:  
  cloud:
    config:
      uri: http://localhost:8888
```


## Spring Boot 2.4 이후

기존에 Spring Cloud Config 를 사용하기 위해서는 bootstrap.yml 에 spring.cloud.config.uri 속성을 사용했습니다.
하지만 Spring Boot 2.4 이후 부터는 bootstrap.yml 에 의한 context 초기화 작업이 지원되지 않습니다.
bootstrap.yml 파일을 사용하기 위해서는 spring.cloud.bootstrap.enabled=true 속성을 사용하거나 spring-cloud-starter-bootstrap 의존성을 추가하여 사용해야합니다.

spring-cloud-starter-bootstrap 의존성을 추가하여 사용하는 방법은 다음과 같습니다.

pom.xml :

``` xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-bootstrap</artifactId>
</dependency>
```

bootstrap.yml :

``` yml
spring:
  config:
    import: optional:configserver: http://localhost:8888
```

bootstrap.yml 을 사용하지 않고 config server uri를 지정하는 방법은 application.yml 을 사용하면 됩니다.

application.yml :

``` yml
spring:
  config:
    import: optional:configserver: http://localhost:8888
```

