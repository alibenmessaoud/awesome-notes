# Docker Test Containers in Java

Testcontainers is a Java library that supports JUnit tests, providing lightweight, throwaway instances of common databases, or anything else that can run in a Docker container.

Testcontainers make the following kinds of tests easier:

- **Data access layer integration tests**: use a containerized instance of a database to test data access layer code without requiring complex setup.
- **Application integration tests**: for running application in a short-lived test mode with dependencies, such as databases, message queues or web servers.
- **UI/Acceptance tests**: use containerized web browsers for conducting automated UI tests using selenium. 

## Prerequisites

Docker, Java, Junit, Maven

## Setup

Open a docker container with all the required tooling: maven, git.

```docker run -it --rm maven:3.5 /bin/bash``` 

Let’s create a default maven project

```shell
mvn archetype:generate \
  -DgroupId=com.testcontainers \
  -DartifactId=app \
  -Dversion=1.0.0-SNAPSHOT \
  -Dpackage=tc.app \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false \
  -DarchetypeCatalog=local
```

Add maven dependency

```xml
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>testcontainers</artifactId>
    <version>1.8.0</version>
</dependency>
```

Do some code … 

## Web Server example

### Java containerized web server 

```java
package tc.app;

import static org.junit.Assert.assertEquals;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.logging.Logger;
import org.junit.ClassRule;
import org.junit.Test;
import org.testcontainers.containers.GenericContainer;

public class AppTest {

   Logger logger = Logger.getLogger(AppTest.class.getName());

   /**
    * @ClassRule // Command to start the container before any test in this
    * class run and will be destroyed in the end;
    */
   /**
    * @Rule // Command to start the container for each method and will destroy
    * the container when a given method ends.
    */
   @ClassRule
   public static GenericContainer container = new GenericContainer("alpine:3.2")
         .withExposedPorts(80)
         .withCommand(
               "/bin/sh", "-c",
               "while true; do echo 'HTTP/1.1 200 OK\n\nHello World!' " +
               "| nc -l -p 80; done"
         );

   public void setUp() {
   }

   public void tearDown() {
   }

   @Test
   public void when_call_get_then_returns_response() throws Exception {

      String address = String.format(
            "http://%s:%s", container.getContainerIpAddress(),
            container.getMappedPort(80)
      );
      logger.info("address: " + address);

      String response = simpleGetRequest(address);
      logger.info("response: " + response);

      assertEquals(response, "Hello World!");
   }

   private String simpleGetRequest(String path) throws Exception {
      HttpURLConnection c = (HttpURLConnection) new URL(path).openConnection();
      c.setRequestMethod("GET");
      InputStreamReader reader = new InputStreamReader(c.getInputStream());
      BufferedReader in = new BufferedReader(reader);
      String inputLine;
      StringBuilder content = new StringBuilder();
      while ((inputLine = in.readLine()) != null) {
         content.append(inputLine);
      }
      in.close();
      return content.toString();
   }
}
```

Console output

```sh
INFO: address: http://localhost:32783
INFO: response: Hello World!
```

## Use Docker compose file containerized web server 

Docker compose files goes as follow:

```bash
server:
  image: alpine:3.2
  command: ["/bin/sh", "-c", "while true; do echo 'HTTP/1.1 200 OK\n\nHello World!' | nc -l -p 80; done"]
```

It is possible to use docker-compose file and declare services instead creating the container programmatically:

```java
   @ClassRule
   public static DockerComposeContainer containerFromDC =
         new DockerComposeContainer(
               new File("src/test/resources/test-compose.yml")
         )
         .withExposedService("server_1", 80);

   @Test
   public void when_call_get_then_returns_response_dc() throws Exception {

      String address = String.format("http://%s:%d",
            containerFromDC.getServiceHost("server_1", 80),
            containerFromDC.getServicePort("server_1", 80)
      );

      String response = simpleGetRequest(address);

      assertEquals(response, "Hello World!");
   }
```

_1 is used to get the service instance as identified with _ a and 1 in line 6;

## Mongo DB

Add maven dependency for Mongo DB

```xml
<dependency>
	<groupId>org.mongodb</groupId>
	<artifactId>mongo-java-driver</artifactId>
	<version>3.10.1</version>
</dependency>
```

The test goes as follows

```java
package tc.app;

import static org.junit.Assert.assertEquals;

import org.bson.Document;
import org.junit.After;
import org.junit.Before;
import org.junit.ClassRule;
import org.junit.Test;
import org.testcontainers.containers.GenericContainer;
import org.testcontainers.containers.wait.strategy.HostPortWaitStrategy;
import com.mongodb.MongoClient;
import com.mongodb.client.MongoCollection;

public class MongoAppTest {

   @ClassRule
   public static GenericContainer mongo = new GenericContainer("mongo:4.1.9")
         .withExposedPorts(27017)
         .waitingFor(new HostPortWaitStrategy());

   @Before
   public void setUp() {
   }

   @After
   public void tearDown() {
   }

   @Test
   public void simpleMongoDbTest() {
      MongoCollection<Document> collection = new MongoClient(
            mongo.getContainerIpAddress(),
            mongo.getMappedPort(27017)
      )
      .getDatabase("test")
      .getCollection("testCollection");

      collection.insertOne(new Document("name", "foo").append("value", 1));

      Document doc2 = collection.find(new Document("name", "foo")).first();
      assertEquals(1, doc2.get("value"));
   }

}
```

