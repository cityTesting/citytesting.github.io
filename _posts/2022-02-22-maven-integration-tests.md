---
layout: post
title: Maven configuration to run integration tests
categories: [maven]
---
When configuring a project to run integration tests it is not enough just to follow the naming convention (ClassNameTest for unit tests and ClassNameTestIT for integration tests). It is necessary to add the path to the folder where the integration tests are located in the pom.xml file:      
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