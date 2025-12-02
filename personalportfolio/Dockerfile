# Stage 1: build with Maven
FROM maven:3.8.8-eclipse-temurin-17 AS build
WORKDIR /app

# copy pom and .mvn to download dependencies first (speeds up builds)
COPY pom.xml ./
COPY mvnw ./
COPY .mvn .mvn
RUN mvn -B -f pom.xml -q dependency:go-offline

# copy source and build the jar
COPY src ./src
RUN mvn -B -f pom.xml -DskipTests clean package

# Stage 2: runtime image
FROM eclipse-temurin:17-jdk
WORKDIR /app
COPY --from=build /app/target/*.jar /app/app.jar
EXPOSE 8080

# Run the jar and let PORT env var control the server port
ENTRYPOINT ["sh","-c","java -jar /app/app.jar --server.port=${PORT:-8080}"]
