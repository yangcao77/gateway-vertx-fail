FROM quay.io/devfile/maven:3.8.1-openjdk-17-slim

WORKDIR /build

# Build dependency offline to streamline build
COPY pom.xml .
RUN mvn dependency:go-offline

COPY src src
RUN mvn package -Dmaven.test.skip=true

FROM openjdk:11-jdk
ENV JAVA_OPTIONS="-Dvertx.cacheDirBase=/tmp/vertx -Djava.io.tmpdir=/tmp"
COPY --from=0 /build/target/gateway-1.0-SNAPSHOT.jar /app/target/gateway-1.0-SNAPSHOT.jar

EXPOSE 8080
ENTRYPOINT [ "java", "-jar", "/app/target/gateway-1.0-SNAPSHOT.jar", "--server.port=8080", "-Dvertx.cacheDirBase=/tmp/vertx", "-Djava.io.tmpdir=/tmp"]