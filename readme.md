# Spring + Vue Demo

## Run

Build frontend:  
>npm must be installed ([with node.js](https://nodejs.org)).

    cd frontend
    npm run build
 
Run backend:

    cd ../backend

on Linux

    ./mvnw clean spring-boot:run

or on Windows

    mvnw.cmd clean spring-boot:run

See result in browser:

    http://localhost:8080

## How it works

### Main idea

Develop frontend separately from backend. After building, copy frontend files into `main/resources/static` folder before compiling backend (with maven).

### How was made

Create frontend for example with [Vue CLI](https://cli.vuejs.org/guide/installation.html) 

    vue create frontend --no-git

Create backend with [spring initializr](https://start.spring.io/). Dependencies: `Spring Web`. Put result in `/backend`.

Frontend files after compiling will have random names (like `app.343da234fa.js`). To clean up obsolete files add goal for **maven-clean-plugin** in `/backend/pom.xml` 

    <!--clear resources/static folder before before copy frontend files-->
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-clean-plugin</artifactId>
        <version>3.1.0</version>
        <configuration>
            <filesets>
                <fileset>
                    <directory>src/main/resources/static</directory>
                </fileset>
            </filesets>
        </configuration>
    </plugin>

Add goal for **maven-resources-plugin** for copy files

    <!--copy frontend files into resources/static-->
    <plugin>
        <artifactId>maven-resources-plugin</artifactId>
        <executions>
            <execution>
                <id>copy vue.js files</id>
                <phase>generate-resources</phase>
                <goals>
                    <goal>copy-resources</goal>
                </goals>
                <configuration>
                    <outputDirectory>src/main/resources/static</outputDirectory>
                    <overwrite>true</overwrite>
                    <resources>
                        <resource>
                            <directory>${project.basedir}/../frontend/dist</directory>
                        </resource>
                    </resources>
                </configuration>
            </execution>
        </executions>
    </plugin>


## Alternatives

* [Cool project](https://github.com/jonashackt/spring-boot-vuejs) with using **maven-frontend-plugin**. Using parent pom. No need to build separately frontend and backend. [Similar to Ru](https://habr.com/ru/post/467161/)
* [Jhipster framework](https://www.jhipster.tech/) for generating web application. It uses **maven-frontend-plugin** too.