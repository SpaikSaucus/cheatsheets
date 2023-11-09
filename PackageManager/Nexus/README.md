# CheatSheets Nexus

## Table of Contents
- [Registry nexus](#registry-nexus)
- [407 Proxy Authentication Required](#407-proxy-authentication-required)
- [Nexus Maven Settings](#nexus-maven-settings)
- [Upload JAR in Nexus](#upload-jar-in-nexus)
- [Download JAR in Nexus](#download-jar-in-nexus)

## Registry nexus
```shell
npm config set registry http://nexus.ar.[subdomain]/repository/npm-public
```

## 407 Proxy Authentication Required
Check if have an old password here:
* C:\Users\[MyUser]\.m2\settings.xml	
* apache-maven\conf\settings.xml
* $MAVEN_HOME/conf/settings.xml
* $M2_HOME/conf/settings.xml


## Nexus Maven Settings
* $MAVEN_HOME/conf/settings.xml
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

## Upload JAR in Nexus

### By Command
```shell
mvn deploy:deploy-file -DgroupId="ar.com.myproject" -DartifactId="myartifact" -Dversion="1.3" -Dpackaging="jar" -Dfile="C:\Users\.m2\repository\ar\com\myproject\myartifact\1.1\myartifact-1.1.jar"  -DrepositoryId="nexus" -Durl=" http://nexus.ar.[subdomain]/repository/myproject-repository/" -DpomFile="C:\Users\.m2\repository\ar\com\myproject\myartifact\1.1\myartifact-1.1.pom"
```
### By UI
![upload_jar](https://github.com/SpaikSaucus/cheatsheets/blob/main/PackageManager/Nexus/Upload_JAR_Nexus.png?raw=true)

## Download JAR in Nexus
![download_jar](https://github.com/SpaikSaucus/cheatsheets/blob/main/PackageManager/Nexus/Download_JAR_Nexus.PNG?raw=true)