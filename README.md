## Git commit sha over Spring boot actuator info [maven project example]

Tired of asking what version is running in QA/Production? Make your life simple through actuator/info and git-commit-id plugin.

If you are in a hurry, there are only four changes you have to make expose git commit sha over actuator/info endpoint

1. Add following dependency to your application - 

```<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency> 
```

2. Enable Spring actuator/info endpoint through adding folowing configration to your application.yml file - 

```management.endpoints.web.exposure.include: info
```

3. Add o.github.git-commit-id plugin 

```<plugin>
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

4. To generate build inforation add an excution block under spring-boot-maven-plugin

```<executions>
      <execution>
         <goals>
            <goal>build-info</goal>
         </goals>
      </execution>
   </executions>
```

