# Use OpenJDK 11 as the base image
FROM openjdk:11-jdk

# Set the working directory
WORKDIR /app

# Copy the Maven project file and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy the project files
COPY src ./src

# Build the application
RUN mvn package -DskipTests

# Run the application
CMD ["java", "-jar", "target/backend-0.0.1-SNAPSHOT.jar"]

# Expose the port
EXPOSE 8080
