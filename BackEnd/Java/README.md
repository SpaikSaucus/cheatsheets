[![en](https://img.shields.io/badge/lang-en-red.svg):ballot_box_with_check:](#) [![es](https://img.shields.io/badge/lang-es-yellow.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/BackEnd/Java/README.es.md)

# CheatSheets Java

## Table of Contents
- [Tutorial](#tutorial)
- [EAR / WAR / JAR](#ear--war--jar)
- [Java Spring Boot](#java-spring-boot)
- [Maven](#maven)
- [Determine JAVA Version in .class](#determine-java-version-in-class)
- [JDK Tools](#jdk-tools)
- [Update JDK](#update-jdk)
- [Similarities with .net](#similarities-with-net)


## Tutorial
* Java SE (Standard edition) 
* Java EE (Enterprise edition)
* JVM (Java Virtual Machine)

The compilation result (bytecodes) is platform independent. The bytecodes are generated in .class files, the JVM at runtime takes the .class and translates them to the operating system it is running on.

The .java file is where we can define the classes of our app, which when compiled will create a .class for each of the classes defined within the .java.
There can only be 1 public class (with the public modifier) within our .java file and the name of said public class must have the same name as the .java file.

#### Reference (SP):
* https://guru99.es/java-tutorial/


## EAR / WAR / JAR

### Differences
The biggest difference between JAR, WAR, and EAR files is the fact that they are targeted at different environments.
* An EAR file requires an application server that fully supports the Java Enterprise Edition (Java EE) or Jakarta Enterprise Edition (EE) platform, such as WebSphere or JBoss, to run.
* A WAR file only requires a Java EE Web Profile-compliant application server to run.
* A JAR file only requires a Java installation.

Some internal restrictions and requirements apply to EAR, WAR, and JAR files.
* EAR files must have an application.xml file contained within a folder called META-INF.
* A WAR file requires a web.xml file contained in a WEB-INF folder.
* A JAR file does not have any of these requirements.
	
### Microservices and JAR files
The current trend in the software development industry is towards microservices development and away from monolithic applications. As such, there has been a move away from developing and deploying enterprise applications deployed as EAR files and a move toward creating smaller components that are deployed as JAR files.

Modern microservices frameworks, such as Spring Boot and Eclipse MicroProfile, deploy applications as executable JAR files that can be deployed directly to a software container, such as Docker, and a container orchestration tool, such as RedHat OpenShift.


## Java Spring Boot
Spring is a framework for developing Java applications. Spring Boot is an extension, a suite, pre-configured to run Spring "out of the box" using best practices and without losing flexibility.

* https://www.ibm.com/topics/java-spring-boot

### Important annotations:
* @Bean :arrow_right: is a singleton.
* @Autowired :arrow_right: inject an instance automatically. 
    * It helps you inject @bean @service @repository into a property of another class. (only used in tests)
* @Service 
* @Repository 
* @Controller 
* @RestController 
* @Transactional
* @GetMapping 
* @PostMapping 
* @Id
* @GeneratedValue(strategy = GenerationType.IDENTITY)


## Maven
Software project understanding and management tool. Based on the concept of the project object model (POM), Maven can manage the construction, reporting, and documentation of a project from one central piece of information.

### Comandos
```shell
mvn -v #to know which version is installed.
mvn install
mvn clean install #The "target" folder is generated that will contain the ".ear" / ".war" / ".jar" file
mvn clean install -DskipTests
mvn run -DskipTests
mvn test
mvn compile
mvn dependency:tree
```

### Build Phases
* validate :arrow_right: Validates that the project is correct and all necessary information is available. This also makes sure the dependencies are downloaded.
* compile :arrow_right: Compiles the source code of the project.
* test :arrow_right: Runs the tests against the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed.
* package :arrow_right: Packs the compiled code in its distributable format, such as a JAR.
* install :arrow_right: Install the package into the local repository, for use as a dependency in other projects locally.
* deploy :arrow_right: Copies the final package to the remote repository for sharing with other developers and projects.

### Settings
* apache-maven\conf\settings.xml
* $MAVEN_HOME/conf/settings.xml
* $M2_HOME/conf/settings.xml
* C:\Users\[MyUser]\.m2\settings.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <servers>
    <server>
      <id>nexus</id>
	  <username>deploy-nexus</username>
	  <password>deploy</password>
    </server>
  </servers>
  <proxies>
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <host>proxy.ar</host>
      <port>8080</port>
      <nonProxyHosts>*.ar.[subdomain]</nonProxyHosts>
    </proxy>
  </proxies>
  <mirrors>
    <mirror>
      <id>maven-public</id>
      <mirrorOf>*</mirrorOf>
      <url> http://nexus.ar.[subdomain]/repository/maven-public</url>
    </mirror>
  </mirrors>
</settings>
```

#### References:
* [Maven web](https://maven.apache.org/)


## Determinate JAVA Version in .class
To determine which version of Java the code was compiled with, we must:
* C:\Users\[myUser]]>cd %JAVA_HOME%
* C:\Program Files\Java\jdk-11.0.8>cd bin
* C:\Program Files\Java\jdk-11.0.8\bin>javap -verbose -cp C:/../[myProject]/target/classes/[nameProject]/*.class | findstr "major"

        Java 1.2 uses major version 46
        Java 1.3 uses major version 47
        Java 1.4 uses major version 48
        Java 5 uses major version 49
        Java 6 uses major version 50
        Java 7 uses major version 51
        Java 8 uses major version 52
        Java 9 uses major version 53
        Java 10 uses major version 54
        Java 11 uses major version 55


## JDK Tools
* javac.exe :arrow_right: to compile
* java.exe :arrow_right: to execute the main (this is automatically used by the IDE) 


## Update JDK
When a new version of the JDK comes out, you must:
* Download the new version.
* Install it.
* Update the JAVA_HOME environment variable with the new jdk path.
* Check the maven command from a console:
    windows+R -> then type -> CMD -> and then execute the following command in the console:
```shell
mvn -v  #to see which Java version is updated. If it did not update, restart the PC.
```
	
* If Visual Studio Code is used, the maven for java extension must be disabled and enabled so that it takes the jdk change. From the VS Code terminal execute:
```shell
mvn -v  #to see which Java version is updated.
```	

#### References:
* [How to check The jdk-version used to compile a class file](https://stackoverflow.com/questions/1096148/how-to-check-the-jdk-version-used-to-compile-a-class-file)


## Similarities with .net 
* The .java is the .cs
* The package is the namespace
   * Classes are organized into packages (directories), with the package statement at the beginning of our .java file
* The main method is the one that starts the program and must be static.
   public static void main(String[] args)
   public static void main(String args[])
   public static void main(String data1, String data2, ....String dataN
* "All java classes inherit from Object"

