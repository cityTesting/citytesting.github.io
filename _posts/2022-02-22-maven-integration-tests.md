---
layout: post
title: Maven configuration to run integration tests
categories: [maven]
---
  
Maven has to main plugins to run tests:  
- maven-surefire-plugin: It is designed to run unit tests. So it will run test with Test in their class name.  
- maven-failsafe-plugin:  It is designed to run integration tests, and will automatically include all test classes with IT in their class name.  

In case that we just uses maven-surefire-plugin we need to that run the integration tests as follow:  

```
  <build>
    <finalName>${project.artifactId}</finalName>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.0.0-M5</version>
        <executions>
          <execution>
            <id>integration-tests</id>
            <phase>integration-test</phase>
            <goals>
              <goal>test</goal>
            </goals>
            <configuration>
              <skipTests>false</skipTests>
              <includes>
                <include>**/integration/**/*.*</include>
              </includes>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
```

After this when we run:

- mvn verify -> Unit and integration tests will be executed
- mvn integration-test -> Unit and integration tests will be executed
- mvn test -> unit tests will be executed
- mvn package -> Unit tests will be executed
- mvn install -> compile tests but they are not executed.
