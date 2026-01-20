# Stage 1: Build
FROM maven:3.8-openjdk-17 AS build
WORKDIR /app

# Копируем только pom для кэширования зависимостей
COPY pom.xml .
RUN mvn dependency:go-offline

# Копируем исходники и собираем jar
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Runtime
FROM eclipse-temurin:17-jre
WORKDIR /

# Копируем собранный jar
COPY --from=build /app/target/*.jar /app.jar

# Указываем точку входа
ENTRYPOINT ["java", "-jar", "/app.jar"]