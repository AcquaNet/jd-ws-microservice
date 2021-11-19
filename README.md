
# JD Web Service Microservice

This tool allow get all information need it from JDE Enterprise Server Manager to generate ini files used by JD Web Service Microservice.




## Prerequisites

 - JDE credential for HTML Client. User, Pasword, Environment and Role.

 - Following folder inside JDE Deployment Server:
   - //Deplo/E920/MISC
   - //Deplo/E920/system/Classes
   - //Deplo/E920/system/JAS/webclient.ear/webclient.war/WEB-INF/lib
   - //Deplo/E920/DV920/java/sbfjars
 
 - JAVA OpenJDK 8

 - Oracle JD Edwards EnterpriseOne Server Manager credential. Ex. http://server:8999/manage/

 - [Docker](  https://docs.docker.com/get-docker/ "Docker"). JDE Web Service Microserver run under Docker container.

 ## JD Generate Ini Files

This tool allow get all information need it from JDE Enterprise Server Manager to generate ini files used by JD Web Service Microservice.

### Installation

Download [JD Generate Ini Files]( http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-create-ini-files/1.0.0/jd-create-ini-files-1.0.0-jar-with-dependencies.jar "Generator") - latest release: 

```
curl http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-create-ini-files/1.0.0/jd-create-ini-files-1.0.0-jar-with-dependencies.jar --output jd-create-ini-files-1.0.0-jar-with-dependencies.jar
```
### Run JD Generate Ini Files
 
```bash
  java -jar jd-create-jar-files-1.0.0-jar-with-dependencies.jar [OPTIONS]

  OPTIONS:                                                                       
  -u, --user[=<user>]         JDE User for Enterprise Server Manager
  -p, --password[=<password>] JDE Password for Enterprise Server Manager
  -s, --server[=<server>]     JDE URL of Server Manager
  -w, --environment[=<server>]     JDE URL of Server Manager

```
Usage Exampes

```bash
java -jar jd-create-jar-files-1.0.0-jar-with-dependencies.jar -u jde_server_manager_user -p password_server_manager -s http://server-manager:8999/manage
```

Output 

```bash
------------------------------------------------------------------------
GENERATION SUCESSS
------------------------------------------------------------------------
 File: \tmp\build_jde_libs\JPS920\jdbj.ini generated
 File: \tmp\build_jde_libs\JPS920\jdeinterop.ini generated
 File: \tmp\build_jde_libs\JPS920\jdelog.properties generated
------------------------------------------------------------------------
```

It will create the following files:

```
build_jde_libs
      ├─ jdbj.ini
      │─ jdeinterop.ini
      └─ jdelog.properties

```

## JD Generate Jars Files

This tool will generate all jars files need it by JD Web Service Microservice.

### Installation

Download [JD Generate Jar Files]( http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-create-jar-files/1.0.0/jd-create-jar-files-1.0.0-jar-with-dependencies.jar "Generator") - latest release: 

```
curl http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-create-jar-files/1.0.0/jd-create-jar-files-1.0.0-jar-with-dependencies.jar --output jd-create-jar-files-1.0.0-jar-with-dependencies.jar
```

#### Preparing folders

Create **/tmp/jde-lib-bundle** folder with the following structure:

```
jde-lib-bundle
      ├─ JDBC_Vendor_Drivers
      └─ system
         │─ Classes 
         │─ JAS    
         └─ WS
 
```

Copy files from JDE Deploment Server to the corresponding folders:
  
        
| Destination                       | Source      | Comments                                                                 |
| -------------------------- | ------------------ | --------------------------------------------------------------------------- |
|JDBC_Vendor_Drivers               |//Deployment Server/E920/MISC/sqljdbc42.jar | JDBC driver will be the corresponding to database used by JD Edwards.|
|system->Classes                   |//Deployment Server/E920/system/Classes\*   | |
|system->JAS                       |//Deployment Server/E920/system/JAS/webclient.ear/webclient.war/WEB-INF/lib/*| |
|system->WS                        | //Deployment Server/E920\DV920/java/sbfjars/* | |

 
#### Create Jars File

```bash
  java -jar jd-create-jar-files-1.0.0-jar-with-dependencies.jar [OPTIONS]

  OPTIONS:                                                                       
  -u, --user[=<user>]         JDE User for Enterprise Server Manager
  -p, --password[=<password>] JDE Password for Enterprise Server Manager
  -s, --server[=<server>]     JDE URL of Server Manager

```
Usage Exampes

```bash
java -jar jd-create-jar-files-1.0.0-jar-with-dependencies.jar -u jde_server_manager_user -p password_server_manager -s http://server-manager:8999/manage
```

Output 

```bash
------------------------------------------------------------------------
GENERATION SUCESSS
------------------------------------------------------------------------
JDE Library bundle has been copied to: C:\tmp\build_jde_libs\jde-lib-wrapped-1.0.0.jar
JDE WS has been copied to: C:\tmp\build_jde_libs\StdWebService-1.0.0.jar
------------------------------------------------------------------------
```

It will create the following files:

```
build_jde_libs
      ├─ jde-lib-wrapped-1.0.0.jar
      └─ StdWebService-1.0.0.jar

```

#### Deploy artifact to Local Repository - Optional

At startup, JD Microservice has the option to get these libraries from an internal repository.
This optional. 

In case you want to use a local repository, exectute the following command:

```
mvn deploy:deploy-file -DgroupId=com.jdedwards -DartifactId=jde-lib-wrapped -Dversion=1.0.0 -DrepositoryId=repo-central -Dpackaging=jar -Dfile=\tmp\build_jde_libs\jde-lib-wrapped-1.0.0.jar -Durl=http://localrepo:8081/artifactory/libs-release
mvn deploy:deploy-file -DgroupId=com.jdedwards -DartifactId=StdWebService -Dversion=1.0.0 -DrepositoryId=repo-central -Dpackaging=jar -Dfile=\tmp\build_jde_libs\StdWebService-1.0.0.jar -Durl=http://localrepo:8081/artifactory/libs-release
```




## Get and Run JD Microservice

### Installation

Download [JD Docker Composer Files]( http://157.245.236.175:8081/artifactory/libs-release/com/atina/jd-docker-files/1.0.0/jd-docker-files-1.0.0.zip "Docker Composer Files") - latest release: 

```
curl http://157.245.236.175:8081/artifactory/libs-release/com/atina/jd-docker-files/1.0.0/jd-docker-files-1.0.0.zip --output jd-docker-files-1.0.0.zip
```

Unzip JD Docker Composer Files (**jd-docker-files-1.0.0.zip**) downloaded in a temporal folder.

```bash
7z e jd-docker-files-1.0.0.zip
```

### Configuration

Edit .env file and change the following values:

#### Customer repository

For Local repository:

| Parameter                | Comment      | 
| -------------------------- | ------------------ |
|CUSTOMER_REPOSITORY_PROTOCOL   |http|
|CUSTOMER_REPOSITORY_URL        |local-repository:8081/artifactory/libs-release|
|JDE_GET_LIB_WRAPPED_UPDATE_FROM_REPOSITORY|1|
|JDE_GET_LIB_WEB_SERVICE_FROM_REPOSITORY|1|

For non Local repository:

| Parameter                | Comment      | 
| -------------------------- | ------------------ |
|CUSTOMER_REPOSITORY_PROTOCOL   ||
|CUSTOMER_REPOSITORY_URL        ||
|JDE_GET_LIB_WRAPPED_UPDATE_FROM_REPOSITORY|0|
|JDE_GET_LIB_WEB_SERVICE_FROM_REPOSITORY|0|

#### dns defintion

| Parameter                | Comment      | 
| -------------------------- | ------------------ |
|JDE_MICROSERVER_ENTERPRISE_SERVER_NAME   |JDE-ENT|
|JDE_MICROSERVER_ENTERPRISE_SERVER_IP        |222.222.222.1|
|JDE_MICROSERVER_ENTERPRISE_DB_NAME|JDE-DB|
|JDE_MICROSERVER_ENTERPRISE_DB_IP|222.222.222.2|


### Create and Run JD Microservice

Run following docker commands:

Create Container

```bash
docker-compose up --no-start
```

Copy files into Container

```bash
docker cp /tmp/build_jde_libs/JPS920 jd-atina-microserver:/tmp/jde/config
docker cp /tmp/build_jde_libs/jde-lib-wrapped-1.0.0.jar jd-atina-microserver:/tmp/jde
docker cp /tmp/build_jde_libs/StdWebService-1.0.0.jar jd-atina-microserver:/tmp/jd
```

Run Container

```bash
docker-compose start
```

Check process

```bash
docker exec -it jd-atina-microserver cat /tmp/start.log
```

```bash
-SERVICE--------------------------------------------
   Name:  172.28.0.2
   Port:  8077
-REPOSITORY-----------------------------------------
   Customer:  http:157.245.236.175:8081/artifactory/libs-release
   Atina:     http:157.245.236.175:8081/artifactory/libs-release
-LIBRARIES------------------------------------------
   StdWebService Version  1.0.0
   jde-lib-wrapped Version 1.0.0
   JDEAtinaServer Version 1.0.0
-LICENSE--------------------------------------------
   Code:  demo
-MICROSERVER----------------------------------------
   JDE_MICROSERVER_TOKEN_EXPIRATION:  3000000
   JDE_MICROSERVER_ENTERPRISE_SERVER_NAME:  JDE-ENT
   JDE_MICROSERVER_ENTERPRISE_SERVER_IP:  122.21.23.261
   JDE_MICROSERVER_ENTERPRISE_DB_NAME:  JDE-DATABASE
   JDE_MICROSERVER_ENTERPRISE_DB_IP:  125.22.139.57
   JDE_MICROSERVER_MOCKING:  0
----------------------------------------------------
ADDITIONAL SCRIPT:
  127.0.0.1       localhost
  ::1     localhost ip6-localhost ip6-loopback
  fe00::0 ip6-localnet
  ff00::0 ip6-mcastprefix
  ff02::1 ip6-allnodes
  ff02::2 ip6-allrouters
  172.28.0.2      bcd05e8e5bd0
----------------------------------------------------
 Check log cat /tmp/jde/JDEConnectorServerLog/jd_atina_server_2021-11-18.0.log
```


```bash
docker exec -it jd-atina-microserver cat cat /tmp/jde/JDEConnectorServerLog/jde_atina_server_2021-11-18.0.log
```

```bash
19:27:06.713 [main] INFO  c.acqua.jde.jdeconnectorserver.JDEConnectorServer - Iniciando JDE Service Impl...
19:27:06.891 [main] INFO  c.a.jde.jdeconnectorserver.server.JDERestServer - *-------------------------------------*
19:27:06.892 [main] INFO  c.a.jde.jdeconnectorserver.server.JDERestServer - *   Starting JDE Microservice 1.0.0   *
19:27:07.061 [main] INFO  c.a.jde.jdeconnectorserver.server.JDERestServer - *   JDE Microservice started!         *
19:27:07.064 [main] INFO  c.a.jde.jdeconnectorserver.server.JDERestServer - *-------------------------------------*
```









 
## JD Docker Composer Files - Environment .env detail
 
### Atina Repository

This repository is used to get updates for JD Microservice.

| Parameter                | Comment      | 
| -------------------------- | ------------------ |
|ATINA_REPOSITORY_PROTOCOL   |Atina repository Protocol|
|ATINA_REPOSITORY_URL        |Atina repository URL |


This is the customer repository is used to artifact:
This is optional.


| Parameter                | Comment      | 
| -------------------------- | ------------------ |
|CUSTOMER_REPOSITORY_PROTOCOL|//Deployment Server/E920/system/JAS/webclient.ear/webclient.war/WEB-INF/lib/*|
|CUSTOMER_REPOSITORY_URL     | //Deployment Server/E920\DV920/java/sbfjars/* |