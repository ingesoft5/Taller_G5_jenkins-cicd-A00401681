# Stage 1: deterministic builder
FROM maven:3.9.6-eclipse-temurin-17-alpine AS builder
WORKDIR /build
COPY pom.xml .
ARG REVISION=0.0.0-local
RUN mvn -B -Drevision="${REVISION}" dependency:go-offline
COPY src ./src
RUN mvn -B -Drevision="${REVISION}" package -DskipTests

# Stage 2: minimal runtime
FROM eclipse-temurin:17-jre-alpine AS runtime
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app
COPY --from=builder /build/target/studytrack-api.jar app.jar
USER appuser
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
