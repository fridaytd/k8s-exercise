# Multi-stage 
FROM maven:3.6.1-jdk-8-alpine AS packaging

RUN mkdir -p /usr/src/app

WORKDIR /usr/src/app

COPY pom.xml /usr/src/app/

RUN mvn -e -B dependency:resolve

COPY . /usr/src/app/

RUN mvn clean package


FROM openjdk:8-jre

# environment
EXPOSE 8084

# executable ADD @project.artifactId@-@project.version@.jar app.jar
COPY --from=packaging /usr/src/app/target/service-two.jar /app.jar

# run app as user 'booter'
RUN /bin/sh -c 'touch /app.jar'
ENTRYPOINT ["java", "-Xmx256m", "-Xss32m", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app.jar"]
