FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Utilisateur non-root (bonne pratique sécurité)
RUN addgroup -S spring && adduser -S spring -G spring

COPY target/*.jar app.jar

RUN chown spring:spring app.jar
USER spring

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
