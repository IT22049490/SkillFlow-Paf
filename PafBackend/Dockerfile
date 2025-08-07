# Stage 1: Build the Spring Boot application
FROM maven:3.9.6-eclipse-temurin-17 AS backend
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

# Stage 2: Run the application using a lightweight JRE
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app

# Copy the jar from the previous build stage
COPY --from=backend /app/target/*.jar app.jar

# Expose the Spring Boot default port
EXPOSE 8081

# Healthcheck (optional)
HEALTHCHECK CMD curl --fail http://localhost:8081/actuator/health || exit 1

# Start the application
ENTRYPOINT ["java", "-jar", "app.jar"]
