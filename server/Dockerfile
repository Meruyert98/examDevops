# Stage 1: Build Stage
FROM maven:3.8.3-openjdk-17 as build

WORKDIR /app

COPY . .

RUN mvn clean install -Dmaven.test.skip=True

# Stage 2: Run Stage
FROM openjdk:17-jdk

WORKDIR /app

COPY --from=build /app/target/back-end-0.0.1-SNAPSHOT.jar app.jar


EXPOSE 8081

ENTRYPOINT ["java", "-jar", "/app/app.jar"]