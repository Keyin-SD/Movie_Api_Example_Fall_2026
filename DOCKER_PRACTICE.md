# Docker Practice: Containerize the Movie Night API

I want you to take this example app and run it in Docker to practice your skills. This doc goes over the steps — I want to make sure people are comfortable doing this.

This is not mandatory to complete but it is a good way to practice your skills, and prepare for the final :)

---

## Getting Started

To get started I like to:

- Make sure Docker is installed and open it on my computer
- Open my project either in an IDE or terminal — the approach you take is up to you
- Then in terminal, make sure I am logged in with Docker using `docker login`

---

## Add the Dockerfile


```dockerfile
# Stage 1: Build the application with Maven + Java 24
FROM maven:3.9-eclipse-temurin-24 AS build
WORKDIR /workspace/app
COPY . .
RUN mvn clean package -DskipTests

# Stage 2: Run the application on Java 24
FROM eclipse-temurin:24-jre-alpine
WORKDIR /app
COPY --from=build /workspace/app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

## Add the Compose File


```yaml
services:
  myapp-main:
    image: <dockerhubusername>/<imagename>:<tag>
    ports:
      - 8080:8080
    environment:
      - spring.datasource.url=jdbc:mysql://<db_url>/<schema_name>
      - spring.datasource.password=<password>
      - spring.datasource.username=<username>
```

^^^ Fill out with your own MySQL username, password, and schema name.

I like to tag my images with my Docker Hub username to avoid any extra step if I need to push the image to my Docker Hub account.

To point to your MySQL locally you need to point to the host machine from Docker. To get to MySQL running on your host machine you set the db url to `host.docker.internal:3306`.

---

## Build and Run

Then I like to build my image using the docker build command in terminal. I like to tag it with my Docker Hub username and the image name and tag.

Command to build the image:

```bash
docker build -t <dockerhubusername>/<imagename>:<tag> .
```

Command to run the image using Docker Compose:

```bash
docker-compose up
```
