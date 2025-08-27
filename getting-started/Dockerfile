# --------------------------
# Stage 1 - Build
# --------------------------
FROM maven:3-eclipse-temurin-21 AS build

WORKDIR /app
COPY . .
RUN mvn package -DskipTests

# --------------------------
# Stage 2 - Runtime
# --------------------------
FROM eclipse-temurin:21-jre-alpine

# Criar um usuário e grupo não-root
RUN addgroup -S quarkus && adduser -S -G quarkus quarkus

WORKDIR /app

# Copiando os arquivos do build para a imagem final
COPY --from=build --chown=quarkus:quarkus app/target/quarkus-app/lib/ /app/lib/
COPY --from=build --chown=quarkus:quarkus app/target/quarkus-app/*.jar /app/
COPY --from=build --chown=quarkus:quarkus app/target/quarkus-app/app/ /app/app/
COPY --from=build --chown=quarkus:quarkus app/target/quarkus-app/quarkus/ /app/quarkus/

EXPOSE 8080
USER quarkus

ENV JAVA_OPTS="-Dquarkus.http.host=0.0.0.0"
ENV JAVA_APP_JAR="quarkus-run.jar"

ENTRYPOINT ["java", "-jar", "quarkus-run.jar"]