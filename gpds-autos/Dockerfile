# Etapa de construcción
FROM maven:3.8.5-openjdk-17 AS build
WORKDIR /app

# Copiar el archivo pom.xml y descargar las dependencias
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copiar el resto del código fuente
COPY . .

# Ejecutar el proceso de construcción
RUN mvn clean package -DskipTests

# Verificar que el archivo .jar se ha generado correctamente
RUN ls -l /app/target/

# Etapa de ejecución
FROM openjdk:17-jdk-slim
WORKDIR /app

# Copiar el archivo .jar desde la etapa build
COPY --from=build /app/target/gpds-autos-0.0.1-SNAPSHOT.jar app.jar

# Exponer el puerto que la aplicación usará
EXPOSE 8083

# Establecer el comando de entrada para ejecutar la aplicación
ENTRYPOINT ["java", "-jar", "app.jar"]
