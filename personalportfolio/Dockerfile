# Stage 1: build with Maven on JDK 21
FROM maven:3.9.9-eclipse-temurin-21 AS build
WORKDIR /app

# copy pom and .mvn to download dependencies first (speeds up builds)
COPY pom.xml ./
COPY mvnw ./
COPY .mvn .mvn
RUN mvn -B -f pom.xml -q dependency:go-offline

# copy source and build the jar
COPY src ./src
RUN mvn -B -f pom.xml -DskipTests clean package

# Stage 2: runtime image (JDK 21)
FROM eclipse-temurin:21-jdk
WORKDIR /app
COPY --from=build /app/target/*.jar /app/app.jar
EXPOSE 8080

# Run the jar and let PORT env var control the server port
ENTRYPOINT ["sh","-c","java -jar /app/app.jar --server.port=${PORT:-8080}"]

