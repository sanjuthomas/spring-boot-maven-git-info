## Git commit sha over Spring boot actuator info [maven project example]

Tired of asking what version is running in QA/UAT/PROD? Make your life simple through the actuator/info and git-commit-id plugin.

There are only four simple changes you have to make to expose git commit sha over actuator/info endpoint.

1. Add following dependency to application's [pom.xml](https://github.com/sanjuthomas/spring-boot-maven-git-info/blob/main/pom.xml#L32)

```
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency> 
```

2. Enable Spring actuator/info endpoint through adding following configuration to [application.yml](https://github.com/sanjuthomas/spring-boot-maven-git-info/blob/main/src/main/resources/application.yaml#L1) file

```
    management.endpoints.web.exposure.include: info
```

3. Add ```io.github.git-commit-id``` plugin to your application [pom.xml](https://github.com/sanjuthomas/spring-boot-maven-git-info/blob/main/pom.xml#L40)

```
    <plugin>
        <groupId>io.github.git-commit-id</groupId>
        <artifactId>git-commit-id-maven-plugin</artifactId>
        <version>5.0.0</version>
        <executions>
          <execution>
            <id>get-the-git-infos</id>
            <goals>
              <goal>revision</goal>
            </goals>
            <phase>initialize</phase>
          </execution>
        </executions>
        <configuration>
          <generateGitPropertiesFile>true</generateGitPropertiesFile>
          <generateGitPropertiesFilename>${project.build.outputDirectory}/git.properties</generateGitPropertiesFilename>
          <includeOnlyProperties>
            <includeOnlyProperty>^git.build.(time|version)$</includeOnlyProperty>
            <includeOnlyProperty>^git.commit.id.(abbrev|full)$</includeOnlyProperty>
          </includeOnlyProperties>
          <commitIdGenerationMode>full</commitIdGenerationMode>
        </configuration>
     </plugin>
```

4. To generate build information add an execution block under ```spring-boot-maven-plugin``` in the [pom.xml](https://github.com/sanjuthomas/spring-boot-maven-git-info/blob/main/pom.xml#L66)

```
    <execution>
        <goals>
            <goal>build-info</goal>
        </goals>
    </execution>   
```

We are done! Check out your application's actuator/info endpoint. You should see a JSON like the following - 

```
   {
        "git":{
            "commit":{
            "id":"6416973"
        }
   },
   "build":{
      "artifact":"spring-boot-maven-git-info",
      "name":"spring-boot-maven-git-info",
      "time":"2022-05-07T23:25:40.010Z",
      "version":"1.0-SNAPSHOT",
      "group":"org.sanjuthomas"
   }
```

