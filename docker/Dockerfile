FROM quay.io/devfile/maven:3.8.1-openjdk-17-slim

WORKDIR /build

# Build dependency offline to streamline build
COPY pom.xml .
RUN mvn dependency:go-offline

COPY src src
RUN mvn package -Dmaven.test.skip=true

FROM openjdk:11-jdk
COPY --from=0 /build/target/gateway-1.0-SNAPSHOT.jar /app/target/gateway-1.0-SNAPSHOT.jar

EXPOSE 8080
ENTRYPOINT [ "java", "-jar", "/app/target/gateway-1.0-SNAPSHOT.jar", "--server.port=8080"]
