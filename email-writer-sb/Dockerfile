# ---- Build Stage ----
FROM eclipse-temurin:21-jdk AS build

WORKDIR /app

# Copy Maven wrapper and config
COPY mvnw .
COPY .mvn .mvn

# Copy pom.xml and download dependencies
COPY pom.xml .
RUN ./mvnw dependency:go-offline || true

# Copy the rest of the source code
COPY . .

# Build the application
RUN ./mvnw clean package -DskipTests

# ---- Runtime Stage ----
FROM eclipse-temurin:21-jdk

WORKDIR /app

# Copy the built jar from the build stage
COPY --from=build /app/target/email-writer-sb-0.0.1-SNAPSHOT.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
