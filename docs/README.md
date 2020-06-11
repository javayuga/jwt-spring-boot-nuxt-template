# Spring Boot JWT demo

## Goals

A project to demonstrate:
 
- JWT authorization with Spring Boot REST API
- Externalized mySQL deployment via docker
- local deployment of Spring Boot app (for development purposes)
- Vuepress as a document building platform

Based on the excellent [Spring Boot tutorial](https://bezkoder.com/spring-boot-jwt-authentication/) by [Bezkoder](https://github.com/bezkoder)

Original Github repository [available here](https://github.com/bezkoder/spring-boot-spring-security-jwt-authentication)

## Ports

Services are listening on ports

| service | url:port | obs
| ---:|:---:|:---
| app      | [localhost:8082](http://localhost:8082) | either after docker-compose or .\mvnw spring:run
| Adminer      | [localhost:8081](http://localhost:8081)|after docker-compose
| mysql | [localhost:3306](http://localhost:3306)| after docker-compose
| vuepress | [localhost:8083](http://localhost:8083)| after yarn docs:dev
      


## JWT

From the [IETF definition](https://tools.ietf.org/html/rfc7519)

> JSON Web Token (JWT) is a compact, URL-safe means of representing claims to be transferred between two parties. 

In-depth information on Spring Security [can be found here](https://docs.spring.io/spring-security/site/docs/current/reference/html5/).


## Dockerized deployment

In Spring Boot, the most common initial approach is to use H2 (in-memory or disk-based) persistence.  That's just fine for prototyping and very small-scale projects.
  
But when you leave the prototyping phase, you'll probably want to work with a more consolidated database, such as mySQL, Postgre, maybe even SQL Server or OracleXE

I decided to use mySQL as a dockerized service.
  
If you have [Docker](https://docs.docker.com/docker-for-windows/) installed on your machine, there is little effort to run a docker-compose script.

This is how the docker-compose.yml looks like:

```
# Use root/example as user/password credentials
version: '3.1'

services:

  db:
    image: mysql:8.0.20
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    ports:
      - 3306:3306
    environment:
      MYSQL_DATABASE: jwt_template
      MYSQL_ROOT_PASSWORD: jwt_template

  adminer:
    image: adminer
    restart: always
    ports:
      - 8081:8080
```

[Adminer](https://www.adminer.org/) is a cute database management app written in PHP, which is a much needed replacement to the aging phpMyAdmin.

## Data initialization with Spring JPA

The data model for this project is quite simple, but there are a few caveats.

Data initialization relies on the Spring Boot pattern of running **resources/data.sql**.  

Here's an excerpt from **resources/application.properties**

```{9,10}
spring.datasource.url= jdbc:mysql://localhost:3306/jwt_template
spring.datasource.username= root
spring.datasource.password= jwt_template

server.port=8082

spring.jpa.properties.hibernate.dialect= org.hibernate.dialect.MySQL5Dialect

spring.datasource.initialization-mode=always
spring.jpa.hibernate.ddl-auto= create-drop
``` 

As indicated by the highlights, data and schema will be refreshed after each restart.

This is good for an example, but might not be appropriate for most use cases.


## Vuepress

[Vuepress](https://vuepress.vuejs.org/) is a static site generator for building documentation of technical projects.

Help for Vuepress configuration options [can be found here](https://vuepress.vuejs.org/config).

Markdown extensions are [described here](https://vuepress.vuejs.org/guide/markdown.htm).





 
