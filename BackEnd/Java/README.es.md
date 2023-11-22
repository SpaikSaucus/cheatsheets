[![en](https://img.shields.io/badge/lang-en-red.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/BackEnd/Java/README.md) [![es](https://img.shields.io/badge/lang-es-yellow.svg):ballot_box_with_check:](#)

# CheatSheets Java

## Tabla de Contenido
- [Tutorial](#tutorial)
- [EAR / WAR / JAR](#ear--war--jar)
- [Java Spring Boot](#java-spring-boot)
- [Maven](#maven)
- [Determinar JAVA Version en .class](#determinar-java-version-en-class)
- [Herramientas JDK](#herramientas-jdk)
- [Actualizar JDK](#actualizar-jdk)
- [Similitudes con .net](#similitudes-con-net) 


## Tutorial
* Java SE (Standard edition) 
* Java EE (Enterprise edition)
* JVM (Maquina virtual java)

El resultado de la compilación (bytecodes) es independiente de la plataforma. Los bytecodes se generan en archivos .class, la JVM en tiempo de ejecución toma los .class y los traduce al sistema operativo en el que esta corriendo.

El archivo .java es donde nosotros podemos definir las clases de nuestra app, la cual al compilar se creara un .class por cada una de las clases definidas dentro del .java.
Solo podrá haber 1 sola clase publica (con el modificador public) dentro de nuestro archivo .java y el nombre de dicha clase publica deberá tener el mismo nombre que el del archivo .java.

#### Referencias:
* https://guru99.es/java-tutorial/


## EAR / WAR / JAR

### Diferencias
La mayor diferencia entre los archivos JAR, WAR y EAR es el hecho de que están dirigidos a diferentes entornos. 
* Un archivo EAR requiere un servidor de aplicaciones totalmente compatible con la plataforma Java Enterprise Edition (Java EE) o Jakarta Enterprise Edition (EE), como WebSphere o JBoss, para ejecutarse. 
* Un archivo WAR solo requiere un servidor de aplicaciones compatible con Java EE Web Profile para ejecutarse.
* Un archivo JAR solo requiere una instalación Java.

También hay restricciones y requisitos internos que se aplican a los archivos EAR, WAR y JAR. 
* Los archivos EAR deben tener un archivo application.xml contenido dentro de una carpeta llamada META-INF. 
* Un archivo WAR requiere un archivo web.xml contenido en una carpeta WEB-INF. 
* Un archivo JAR no tienen ninguno de estos requisitos.
	
### Microservicios y archivos JAR
La tendencia actual en la industria del desarrollo de software es hacia el desarrollo de microservicios y lejos de las aplicaciones monolíticas. Como tal, ha habido un alejamiento del desarrollo y la implementación de aplicaciones empresariales implementadas como archivos EAR y un movimiento hacia la creación de componentes más pequeños que se implementan como archivos JAR.

Los marcos de microservicios modernos, como Spring Boot y Eclipse MicroProfile, implementan aplicaciones como archivos JAR ejecutables que se pueden implementar directamente en un contenedor de software, como Docker, y en una herramienta de orquestación de contenedores, como RedHat OpenShift.	


## Java Spring Boot
Spring es un framework para desarrollo de aplicaciones Java. Spring Boot es una extesión, un suite, pre-configurado para ejecutar Spring "fuera de la caja" utilizando las mejores prácticas y sin perder flexibilidad.

* https://www.ibm.com/mx-es/topics/java-spring-boot

### Annotations importantes:
* @Bean :arrow_right: es un singleton.
* @Autowired :arrow_right: inyecta una instancia automáticamente. 
  * Te sirve para inyectar @bean @service @repository en una property de otra clase. (solo se usa en los test)
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
Herramienta de comprensión y gestión de proyectos de software. Basado en el concepto de modelo de objetos de proyecto (POM), Maven puede gestionar la construcción, los informes y la documentación de un proyecto desde una pieza de información central.

### Comandos
```shell
mvn -v #para saber que versión esta instalada.
mvn install
mvn clean install #se genera la carpeta "target" que contendrá el archivo ".ear" / ".war" / ".jar"
mvn clean install -DskipTests
mvn run -DskipTests
mvn test
mvn compile
mvn dependency:tree
```

### Fases de build
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

#### Referencias:
* [Maven web](https://maven.apache.org/)


## Determinar JAVA Version en .class
Para determinar con que version de java se compilo el código debemos:
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


## Herramientas JDK
* javac.exe :arrow_right: para compilar
* java.exe :arrow_right: para ejecutar el main (esto lo usa el IDE automáticamente) 


## Actualizar JDK
Cuando salga una nueva version del JDK, se debe:
* Descargar nueva version.
* Instalarla.
* Actualizar la variable de entorno JAVA_HOME con la nueva ruta del jdk.
* Chequear el comando de maven desde una consola:
	windows+R -> luego escribir -> CMD -> y luego ejecutar en la consola el siguiente comando:
```shell
mvn -v  #para visualizar que Java version esta actualizado. En caso de que no se actualizo, realizar el reinicio de la pc.
```
	
* Si se utiliza el Visual Studio Code, se debe deshabilitar y habilitar la extension maven for java, para que tome el cambio del jdk. Desde la terminal del VS Code ejecutar:
```shell
mvn -v  #para visualizar que Java version esta actualizado.
```
	
#### Referencias:
* [How to check The jdk-version used to compile a class file](https://stackoverflow.com/questions/1096148/how-to-check-the-jdk-version-used-to-compile-a-class-file)


## Similitudes con .net 
* El .java es el .cs  
* El package es el namespace 
  * Las clases se organizan en paquetes (directorios), con la sentencia package al principio de nuestro archivo .java
* El método main es el que inicia el programa y debe ser estático. 
  public static void main(String[] args)
  public static void main(String args[])
  public static void main(String data1, String data2, ....String dataN
* "Todas las clases java heredan de Object"