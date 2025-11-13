# Multi-stage Dockerfile: build with Maven, then copy the artifact into a smaller runtime image

# Build stage: compile the application and produce a jar
# Build stage: use an official Maven image based on Temurin JDK 11
# (these tags are commonly available on Docker Hub)
FROM maven:3.9.4-eclipse-temurin-11 AS build
WORKDIR /src

# copy pom first to leverage Docker cache for dependencies
COPY pom.xml ./
COPY src ./src

# package the application (skip tests for speed). This creates target/*.jar
RUN mvn -B -DskipTests package

# Runtime stage: use Temurin JRE 11
FROM eclipse-temurin:11-jre
WORKDIR /opt/app

# Copy the built jar from the build stage. Use a wildcard in case the artifact name changes.
COPY --from=build /src/target/*.jar app.jar

ENTRYPOINT ["java","-jar","app.jar"]
