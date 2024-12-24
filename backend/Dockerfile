# Use an official OpenJDK image
FROM openjdk:21-jdk-slim

# Set the working directory inside the container
WORKDIR /app

# Copy the jar file and keystore into the container
COPY backend-0.0.1-SNAPSHOT.jar app.jar

# Expose the SSL port
EXPOSE 8080

# Command to run the Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]
