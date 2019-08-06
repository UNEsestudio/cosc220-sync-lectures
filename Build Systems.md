class: center, middle

# Build Systems
---

## A programs get larger ...

* When there are relatively few files involved we can invoke a compiler directly

  ```sh
  javac -cp mylib.jar cosc220/App.java  
  ```

  But when there are *dozens* of source files, *dozens* of libraries, lots of flags to set, that becomes unwieldy.
  And there are other things to do such as managing where we keep our libraries

--

* You're not the only one building the code:

  * ~60 other team members

  * +Jenkins 

--

* So, it can save a lot of effort if there's a *short*, *simple* command to do everything involved in building the program. *Build systems* help us to do that.

---

class: center, middle

## A brief history of build systems

---

## Make (1976) - Build targets

The build system for the C programming language. It was inspired by a programmer wasting a morning trying to debug a bug that had already been fixed but not compiled.

--

* Usually, we have more than one thing we want to build.  
  *(e.g., modules, the executable, the documentation)*

* Let's call something we want to build a **build target**

* Some targets might depend on other targets  
  *(e.g., the executable depends on the modules)*

--

We want the build system to build the things that have changed, but not waste time re-building things it doesn't need to.

---

## Makefile

A file called `Makefile` describes the build targets, what they depend on, and how to build them

```Makefile
CC = gcc
CFLAGS = -g

all: helloworld

helloworld: helloworld.o
    $(CC) $(LDFLAGS) -o $@ $^

helloworld.o: helloworld.c
    $(CC) $(CFLAGS) -c -o $@ $^

clean: FRC
    rm helloworld *.o

FRC:
```

Let's break this down on the next slides...

---

## Makefile 

A file called `Makefile` describes the build targets, what they depend on, and how to build them

```Makefile
CC = gcc      # Means that when we say $(CC) we mean run gcc
CFLAGS = -g   # Sets flags that we'll pass to the compiler

all: helloworld

helloworld: helloworld.o
    $(CC) $(LDFLAGS) -o $@ $^

helloworld.o: helloworld.c
    $(CC) $(CFLAGS) -c -o $@ $^

clean: FRC
    rm helloworld *.o

FRC:
```

The top two lines set options for how we build the code later

---


## Makefile 

A file called `Makefile` describes the build targets, what they depend on, and how to build them

```Makefile
CC = gcc     
CFLAGS = -g  

all: helloworld   # "all" is a target. It depends on the helloworld executable

helloworld: helloworld.o
    $(CC) $(LDFLAGS) -o $@ $^

helloworld.o: helloworld.c
    $(CC) $(CFLAGS) -c -o $@ $^

clean: FRC
    rm helloworld *.o

FRC:
```

If we say `make all`, it will only rebuild the `helloworld` executable if some of the things it depends on have changed since it was last built.

---


## Makefile 

A file called `Makefile` describes the build targets, what they depend on, and how to build them

```Makefile
CC = gcc     
CFLAGS = -g  

all: helloworld   

helloworld: helloworld.o         # The executable depends on the .o file
    $(CC) $(LDFLAGS) -o $@ $^    # This is how to build it

helloworld.o: helloworld.c
    $(CC) $(CFLAGS) -c -o $@ $^

clean: FRC
    rm helloworld *.o

FRC:
```

The executable will only be rebuilt if the modules it is made from (here, just `helloworld.o`) have updated or need to be rebuilt

---

## Makefile 

A file called `Makefile` describes the build targets, what they depend on, and how to build them

```Makefile
CC = gcc     
CFLAGS = -g  

all: helloworld   

helloworld: helloworld.o         
    $(CC) $(LDFLAGS) -o $@ $^    

helloworld.o: helloworld.c       # The .o file depends on the C source file
    $(CC) $(CFLAGS) -c -o $@ $^  # This invokes the compiler

clean: FRC
    rm helloworld *.o

FRC:
```

Now Make knows how to build the code all the way from the C source to the executable. And it will only re-build the parts it needs to.

---

## Makefile 

A file called `Makefile` describes the build targets, what they depend on, and how to build them

```Makefile
CC = gcc     
CFLAGS = -g  

all: helloworld   

helloworld: helloworld.o         
    $(CC) $(LDFLAGS) -o $@ $^    

helloworld.o: helloworld.c       
    $(CC) $(CFLAGS) -c -o $@ $^ 

clean: FRC              # The "clean" target lets us clean up all generated files
    rm helloworld *.o   # This is how to do it

FRC:                    # It depends on an empty pseudo-target
```

Let's demo ...

---

### Maven (2001) - Projects have structure

Most Java projects have a fairly similar structure:

```
src/
    main/
        java/
            mypackage/
                MyClass.java
        resources/
            mypackage/
                myimage.png
    test/
        java/
            mypackage/
                MyClassTest.java
        resources/
            mypackage/
                mytestimage.png
```

Maven expects Java projects to follow this convention, so it doesn't need to be configured.

---

### Maven (2001) - Dependency management

In times gone by, developers used to have to download their libraries manually

* How to ensure everyone on the team has the same version?

* Some teams checked libraries into version control, but this makes the version control repository grow large

--

Solution - 

* Declare the library in the build file, and let the build system fetch it for you

    ```xml
    <dependency>
      <groupId>org.picocontainer</groupId>
      <artifactId>picocontainer</artifactId>
      <version>2.8</version>
    </dependency>
    ```

---

### Maven (2001) - Artifact repositories

Maven needs to know where to download the libraries from.
They typically have a separate URL for people to search using a UI, and a URL that Maven uses to download artifacts.

* Maven Central  
  search UI: https://search.maven.org  
  Maven URL: http://repo.maven.apache.org/maven2/  

* JCenter  
  search UI: https://bintray.com/bintray/jcenter  
  Maven URL: https://jcenter.bintray.com/  
  
---

### Maven (2001) - Proxy repositories

If you're behind a web proxy that requires a username and password to make requests *out* to the internet, it can be painful to configure Maven to make those requests.

Some companies (and UNE) host a "proxy repository" - a repository inside their network that will mirror on demand libraries from well known public repositories

Ours is Artifactory on hopper    

* search UI: https://hopper.une.edu.au/artifactory
* Maven URL: https://hopper.une.edu.au/artifactory/libs-release/

---

### Maven (2001) - Build lifecycle

If the build system has to download your libraries before it can compile your code, that suggests there are some standard phases to building a project. e.g.:

1. Resolving dependencies for the compile phase
2. Downloading dependencies
3. Compiling the code
4. Resolving dependencies for any automated tests you want to run
5. Downloading test dependencies
6. Compiling the test code
7. Running the tests

(That's not an exhaustive list)

---

### But - XML is not an easy read

```xml
<?xml version="1.0" encoding="utf-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <artifactId>robocode.core</artifactId>
    <name>Robocode Core</name>
    <parent>
        <groupId>au.edu.uq.csse2003</groupId>
        <artifactId>robocode</artifactId>
        <version>2012.0.1-SNAPSHOT</version>
    </parent>
    <dependencies>
        <dependency>
            <groupId>au.edu.uq.csse2003</groupId>
            <artifactId>robocode.api</artifactId>
            <version>${project.version}</version>
        </dependency>
        <!-- container -->
        <dependency>
            <groupId>org.picocontainer</groupId>
            <artifactId>picocontainer</artifactId>
            <version>2.8</version>
        </dependency>
        <!-- test scoped -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.9</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-resources-plugin</artifactId>
                <executions>
                    <execution>
                        <id>copy-resources</id>
                        <phase>validate</phase>
                        <goals>
                            <goal>copy-resources</goal>
                        </goals>
                        <configuration>
                            <outputDirectory>${basedir}/target/classes</outputDirectory>
                            <resources>
                                <resource>
                                    <directory>..</directory>
                                    <filtering>false</filtering>
                                    <includes>
                                        <!-- actually this is bit more complicated than usual, because of quirks with IDEA -->
                                        <include>versions.txt</include>
                                    </includes>
                                </resource>
                            </resources>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

---

## Gradle (2006) - Buildfiles are programs

Gradle is a very flexible build system that - 

* Uses Groovy or Kotlin programming languages to define buildfiles. 

* Lets you define tasks and what they depend on

* Allows plug-ins to define conventions and tasks 
  *(e.g., the Java plugin expects a Maven-like file structure)*

* Supports fetching libraries from the web

---

## A note on versions 

* Gradle 5 differs slightly from Gradle 4. 

* Generally, gradle 5 seems easier - some things that were awkward in 4 seem to be simpler now.

* Be alert that occasionally you might see a Gradle 4 example! To tell which version of gradle you are using:

  ```sh
  gradle -version
  ```

---

## Gradle 5 build.gradle

```gradle
plugins {
    id 'java'
    id 'application'
}

repositories {
    jcenter()
}

dependencies {
    implementation 'com.google.guava:guava:27.1-jre'
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.4.2'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.4.2'
}

application {
    mainClassName = 'cosc220.App'
}

test {
    useJUnitPlatform()
}
```

---

### Plugins

```gradle
plugins {
    id 'java'
    id 'application'
}
```

The `java` plugin defines this is a Java project.  
This defines tasks such as `gradle compile`, `gradle test`, and `gradle build`

The `application` plugin defines the `gradle run` task. It requires the main class to be set:

```gradle
application {
    mainClassName = 'cosc220.App'
}
```

---

### Repositories

* The sample used a well-known repository

    ```gradle
    repositories {
      jcenter()
    }
    ```

* But we'd need to define Artifactory on hopper

    ```gradle
    repositories {
      maven { url "https://hopper.une.edu.au/artifactory/libs-release/" }
      maven { url "https://hopper.une.edu.au/artifactory/libs-snapshot/" }
    }
    ```

---

### Dependencies

Dependencies are declared in their own block. (This code is from a generated sample)

```gradle
dependencies {
    // This dependency is exported to consumers, that is to say found on their compile classpath.
    api 'org.apache.commons:commons-math3:3.6.1'

    // This dependency is used internally, and not exposed to consumers on their own compile classpath.
    implementation 'com.google.guava:guava:27.0.1-jre'

    // Use JUnit test framework
    testImplementation 'junit:junit:4.12'
}
```

---

### JavaFX and Gradle

To use Java 11 and JavaFX, we need to use Gradle 5. 

The JDK no longer bundles the JavaFX UI libraries. Instead, a gradle plugin is going to get them for us.

```gradle
plugins {
  id 'application'
  id 'org.openjfx.javafxplugin' version '0.0.7'
}
```

The javafxplugin then requires us to say which version of javafx we're using, and which modules we want from it

```gradle
javafx {
    version = "11"
    modules = [ 'javafx.controls' ]
}
```

---

### JavaFX and Gradle - a wrinkle

By default, Gradle would try to get the JavaFX plugin from the Gradle Plugin Portal. Unfortunately, the web proxy would get in its way. So, we have to tell Gradle to get its plugins from Artifactory on hopper too.


*settings.gradle*

```gradle
pluginManagement {
  repositories {
    maven { url "https://hopper.une.edu.au/artifactory/gradle-plugins/" }
    gradlePluginPortal() // Include public Gradle plugin portal
  }
}
```


---

### Multi-module builds

* Our project is going to have a client, and it will have a server. That's two executables

* We probably also want some code that is common to both the client and the server. 

--

* Gradle allows us to define *multi-module projects*. 

    ```
    project/
        build.gradle
        client/
            build.gradle
            src/
        shared/
            build.gradle
            src/
        server/
            build.gradle
            src/
        settings.gradle
    ```

---

### Multi-module builds (preview)

* In the top level `build.gradle` we can make declarations across projects. e.g.

    ```gradle
    subprojects {
        repositories {
            maven { url "https://hopper.une.edu.au/artifactory/libs-release/" }
        }
    }
    ```

--

* In `settings.gradle` we list the directories that are subprojects 

    ```gradle
    include "shared", "server", "client"
    ```

--

* The subprojects can then declare dependencies on each other. E.g.

    ```gradle
    dependencies {
        implementation project(':shared')
    }
    ```

---

## Building multi-project builds

Gradle will run tasks in the directory you are in and any subprojects lower.

* `gradle build` in the top-level project will compile all the subprojects

* `gradle build` in the `shared` project will just compile the `shared` project

But it will also always evaluate if it needs to build something else first. e.g.,

* `gradle build` in the `client` project will cause the `shared` project to be compiled if it needs to be.


---

class: bottom

Written by Will Billingsley while at NICTA, UQ, and UNE

<small>
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.</small>