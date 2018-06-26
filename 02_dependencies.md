# 2 - Dependencies

Application's dependencies must be declared and isolated

## What does that mean for our application ?

The example voting application is a polyglot application that uses java, python and node,js to implement the services that make up the application. Declaration are done in pom.xml, requirements.txt and package.json file.

Let's look at the dependencies in each service.

The worker service uses [pom.xml](example-voting-app/worker/pom.xml) declaring dependencies and plugins needed to build the application.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>worker</groupId>
  <artifactId>worker</artifactId>
  <version>1.0-SNAPSHOT</version>

  <dependencies>
    <dependency>
      <groupId>org.json</groupId>
      <artifactId>json</artifactId>
      <version>20140107</version>
    </dependency>

    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>2.7.2</version>
        <type>jar</type>
        <scope>compile</scope>
    </dependency>

    <dependency>
        <groupId>org.postgresql</groupId>
        <artifactId>postgresql</artifactId>
        <version>9.4-1200-jdbc41</version>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>2.4</version>
        <configuration>
          <finalName>worker</finalName>
          <archive>
            <manifest>
              <addClasspath>true</addClasspath>
              <mainClass>worker.Worker</mainClass>
              <classpathPrefix>dependency-jars/</classpathPrefix>
            </manifest>
          </archive>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <configuration>
          <source>1.7</source>
          <target>1.7</target>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <goals>
              <goal>attached</goal>
            </goals>
            <phase>package</phase>
            <configuration>
              <finalName>worker</finalName>
              <descriptorRefs>
                <descriptorRef>jar-with-dependencies</descriptorRef>
              </descriptorRefs>
              <archive>
                <manifest>
                  <mainClass>worker.Worker</mainClass>
                </manifest>
              </archive>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project
```

The vote service is written in python and dependencies are declared in [requirements.txt](example-voting-app/vote/requirements.txt)

```txt
Flask
Redis
gunicorn
```

The results service is written in node.js, requirements are declared in [package.json](example-voting-app/result/package.json)

```json
{
  "name": "result",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "MIT",
  "dependencies": {
    "body-parser": "^1.14.1",
    "cookie-parser": "^1.4.0",
    "express": "^4.13.3",
    "method-override": "^2.3.5",
    "async": "^1.5.0",
    "pg": "^4.4.3",
    "socket.io": "^1.3.7"
  }
}
```

As you can see, each application uses a language specific format but they all perform the same function.

[Previous](01_codebase.md) - [Next](03_configuration.md)
