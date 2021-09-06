FROM docker.io/maven:3.6.3-openjdk-8-slim AS cicdsimulation

RUN apt-get update && apt-get install --no-upgrade -y git

ADD https://raw.githubusercontent.com/kkenan/basic-microservices/master/spring-boot-app/pom.xml /tmp/app/pom.xml
WORKDIR /tmp/app

RUN mvn dependency:go-offline

ADD https://api.github.com/repos/kkenan/basic-microservices/contents/spring-boot-app /dev/null
RUN git clone https://github.com/kkenan/basic-microservices.git /tmp/basic-microservices

COPY application-compose.yaml /tmp/basic-microservices/spring-boot-app/src/main/resources/application-compose.yaml

WORKDIR /tmp/basic-microservices/spring-boot-app
RUN mvn clean package

FROM adoptopenjdk/openjdk8:x86_64-alpine-jre8u302-b08 AS runtime

EXPOSE 8080

ENV JAVA_TOOL_OPTIONS='-Xms512M -Xmx512M'

RUN addgroup -S java && adduser -S java -G java

USER java

COPY --from=cicdsimulation /tmp/basic-microservices/spring-boot-app/target/spring-boot-app-*.jar /home/java/spring-app.jar

ENTRYPOINT ["java","-jar", "-Dspring.profiles.active=compose", "/home/java/spring-app.jar"]