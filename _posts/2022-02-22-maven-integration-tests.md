---
layout: post
title: Maven configuration to run integration tests
categories: [maven]
---
  
Maven has to main plugins to run tests:  
- maven-surefire-plugin: It is designed to run unit tests, it will run tests inside classes with "Test" in their class name.  
- maven-failsafe-plugin:  It is designed to run integration tests, it will run tests inside classes with "IT" in their class name.  

In case that we just use maven-surefire-plugin we need to do add the folder that contains the integration tests, in other case it will run just the unit tests.  

{% highlight xml %} 
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
{% endhighlight %}


After this when we run:

- mvn verify → Unit and integration tests will be executed
- mvn integration-test → Unit and integration tests will be executed
- mvn test → Unit tests will be executed
- mvn package → Unit tests will be executed
- mvn install → Compile tests but they are not executed.


In case that we want to use maven-failsafe-plugin we need to do this to include the integration tests:  
{% highlight xml %} 
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-failsafe-plugin</artifactId>
        <version>3.0.0-M5</version>
        <executions>
          <execution>
            <id>integration-tests</id>
            <goals>
              <goal>integration-test</goal>
              <goal>verify</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>

{% endhighlight %}  

After this when we run:

- mvn verify → Unit and integration tests will be executed.
- mvn integration-test → Unit and integration tests will be executed.
- mvn test → Unit tests will be executed.
- mvn package → Unit tests will be executed.
- mvn install → Compile tests but they are not executed.
- mvn failsafe:integration-test → Integration tests will be executed.
