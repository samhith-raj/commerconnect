# Use the official OpenJDK image as the base image
FROM openjdk:11-jdk

# Install Maven
RUN apt-get update && apt-get install -y maven

# Set the working directory inside the container
WORKDIR /app

# Copy the Maven project file
COPY pom.xml .

# Download the project dependencies
RUN mvn dependency:go-offline -B

# Copy the project files
COPY src ./src

# Build the project
RUN mvn clean package -DskipTests

# Expose the port the app runs on
EXPOSE 8080

# Run the application
CMD ["java", "-jar", "target/backend-0.0.1-SNAPSHOT.jar"]