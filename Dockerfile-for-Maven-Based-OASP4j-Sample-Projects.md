Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.

## Use multi-stage builds
  In Docker, one of the main issues is the size of the final image. It’s not uncommon to end up with images over 1 GB even for simple Java applications. Since version 17.05 of Docker, it’s possible to have multiple builds in a single Dockerfile, and to access the output of the previous build into the current one. Those are called multi-stage builds. The final image will be based on the last build stage.

Let’s imagine the code is hosted on GitHub, and that it’s based on Maven. Build stages would be as follows:

*  Clone the code from GitHub.
* Copy the folder from the previous stage; build the app with Maven.
* Copy the JAR/WAR from the previous stage; run it with java -JAR/WAR .

### Create , Build and Run the Dockerfile. 

#### 1. Create the docker file. 
This docker build file can be used for building any web app with the following features:

*   The source code is hosted on GitHub.
*   The build tool is Maven.
*   The resulting output is an executable JAR/WAR file.


```
FROM alpine/git as clone
ARG url (1)
WORKDIR /app
RUN git clone ${url} (2)
FROM maven:3.5-jdk-8-alpine as build
ARG project (3)
WORKDIR /app
COPY --from=clone /app/${project} /app
RUN mvn install
FROM openjdk:8-jre-alpine
ARG artifactid
ARG version
ENV artifact ${artifactid}-${version}.jar (4)
WORKDIR /app
COPY --from=build /app/target/${artifact} /app
EXPOSE 8080
CMD ["java -jar ${artifact}"] (5)
```
```
1 url must be passed on the command line to set which GitHub repo to clone
2 url is replaced by the passed value
3 Same as <1>
4 artifact must be an ENV, so as to be persisted in the final app image
5 Use the artifact value at runtime
```


#### Here is a sample dockerfile build file to start from:

```
FROM alpine/git
WORKDIR /app
RUN git clone RUN git clone https://github.com/Himanshu122798/mtsj.git (1)
FROM maven:3.5-jdk-8-alpine
WORKDIR /app
COPY --from=0 /app/mtsj /app (2)
RUN mvn install (3)
FROM openjdk:8-jre-alpine
WORKDIR /app
COPY --from=build /app/server/target/mtsj-server-bootified.war /app (4)
EXPOSE 8080
ENTRYPOINT ["sh", "-c"]  
CMD ["java -jar mtsj-server-bootified.war"] (5)
```
It maps the above build stages:

```
1. Clone the Spring maven project git repository from Github.
2. Copy the project folder from the previous build stage
3. Build the app
4. Copy the JAR from the previous build stage
5. Run the app
```
## Build a Docker Image
Build the Docker image using following commands including the dot.

```
docker build -f Dockerfile -t mtsj .
```
```
-t – for tagging the image. (in this example tag is mtsj).
-f – point to a Docker file location.
```
## Run Docker Image
Use this command to run the Spring Boot application.
```
docker run -p 8080:8080 mtsj
```



