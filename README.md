
# JD ATINA Microservices

JD ATINA Microservices is made up of a series of the following services:

* JD Atina Web Service Microservice
* JD Atina Rest-API Microservice
* JD Atina Mule 3/4 CE Connector

![Main Image](images/main.png "Main")


## JD ATINA Web Service Microservice

It's a simple Microservice used to consume JDE Web Services directly to Enterprise Service Logic instead use JDE Applications Server.

It allows third party sofware to connect to JDE Web Services to share logic and data.

It provides:

* Access to JDE Web Services.

* Session management.

* Connection pooling.

* Metadata introspection.

## JD ATINA Rest API Microservice

It's a simple Microservice used to epose JDE Web Services as Rest API.

Any third party sofware with HTTP Connector can consume WS.

![Main Image](images/rest-api.png "Rest API Server")

## JD Atina Mule 3/4 CE Connector

It's a Mule CE connector to consume Web Service from Mulesoft Applications.

![Main Image](images/MuleConnector.png "Mule JDE Connector")

## Implementations

The following implementation steps need to be performed before working with JD Web Service Microservice:

* Set up a configuration

* Start JD Web Services Microservice / JDE Rest API Microservice

* Install Mulesoft JDE WS Community Edition Connector 3.



## Prerequisites

 - Oracle JD Edwards EnterpriseOne HTML Client credential: User, Pasword, Environment and Role.

 - Oracle JD Edwards EnterpriseOne Server Manager credential: jde admin user, Pasword and url for Server Manager Ex. http://server:8999/manage/

 - Following folder inside JDE Deployment Server:
   - //Deplo/E920/MISC
   - //Deplo/E920/system/Classes
   - //Deplo/E920/system/JAS/webclient.ear/webclient.war/WEB-INF/lib
   - //Deplo/E920/DV920/java/sbfjars

 - [Docker](  https://docs.docker.com/get-docker/ "Docker"). JDE Web Service Microserver run under Docker container.

 
## Addionals Prerequisites to use JD Generate Configuration Files and JD Generate Jars Files Tools from java version. 

It not requires for docker version.

 - JDK 8 installed with JAVA_HOME configured appropriately

 - Apache Maven 3.8.1+.   

> ![Tip](images/note-icon.png) 
                        Verify Maven is using the Java you expect
                        If you have multiple JDK???s installed it is not certain Maven will pick up the expected java and you could end up with unexpected results. 
                        You can verify which JDK Maven uses by running mvn --version.
 
 # Set up a configuration

## JD Generate Configuration Files Tool

![Configuration File Tool Image](images/IniTool.png "Configuration File Tool")

JD WS Microservice needs the following ini files according to your environment:

* jdbj.ini

* jdeinterop.ini

* jdelog.properties

* tnsnames.ora" (for Oracle RDBMS based installations only)

You can configure it manually following JDE manuals or you can use this tools to help you.

This tool help to you to generate a base ini files. 

It takes all information from JDE Enterprise Server Manager using [REST API for Server Manager](https://docs.oracle.com/en/applications/jd-edwards/cross-product/9.2/eoira/ "REST API for Server Manager") 

The tooll will require to select the HTML Client instance according to your environment. 

> ![Tip](images/note-icon.png) 
                        At the end of this process, We recommend review jdeinterop.ini and jdbj.ini files 
                        before deploy it on JD Web Service Microservice using this guide:
                        * [Understanding jdeinterop.ini File]( https://docs.oracle.com/cd/E24705_01/doc.91/e24221/jdeinterop_java.htm#EOTCN00333 "Understanding jdeinterop.ini File")

### Using JAVA APP

It will require openjdk 8.

Download [JD Generate Configuration Files]( http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-create-ini-files/1.0.0/jd-create-ini-files-1.0.0-jar-with-dependencies.jar "Generator") - latest release: 

```
curl http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-create-ini-files/1.0.0/jd-create-ini-files-1.0.0-jar-with-dependencies.jar --output jd-create-ini-files-1.0.0-jar-with-dependencies.jar
```
### Run JD Generate Ini Files Tool
 
```bash
  java -jar jd-create-ini-files-1.0.0-jar-with-dependencies.jar [OPTIONS]

  OPTIONS:                                                                       
Options category 'startup':
  --debug [-d] (a string; default: "N")
    Debug Option
  --environment [-e] (a string; default: "")
    JDE Environment
  --password [-p] (a string; default: "")
    JDE Password for Enterprise Server Manager
  --server [-s] (a string; default: "")
    JDE URL of Server Manager
  --user [-u] (a string; default: "")
    JDE User for Enterprise Server Manager

```
Usage Exampes

```bash
java -jar jd-create-ini-files-1.0.0-jar-with-dependencies.jar -u jde_admin -p XXXXXXX -s http://server-manager:8999/manage -e JDV920

```

### Using Docker

```bash
docker run --rm -v /tmp/build_jde_libs:/tmp/build_jde_libs/ -i --name jd-create-ini-files 92455890/jd-create-ini-files:1.0.0 jde_admin jde_password JPS920 http://servermanager.com:8999/manage
```

### Reviewing Output

```bash
Folder : /tmp/build_jde_libs/JPS920 has been created

Authenticating : http://server-manager:8999/manage
         Cookie: 044GXl43PnHkTe1L3AMc/siNkFOIoll+S4ngsdGnNkS7Qg=MDA5MDE1MDE1amRlX2FkbWluMTM0LjIwOS4yMTEuMjQ4MTM0LjIwOS4yMTEuMjQ4MTYzNzYwNzAxMTIzOQ==
Getting Server Groups : http://server-manager:8999/manage

Select HTML Instance for environment JPS920:
0 - alpha_db
1 - alpha_dep
2 - alpha_dv920
3 - alpha_ent
4 - html_ps920
Q - Quit
4
Option Selected: 4

JDE Instance selected: html_ps920

Getting Instance Values: http://server-manager:8999/manage for Instance Name 'html_ps920'
 Processing File: jdbj.ini
 Processing File: jdeinterop.ini
 Processing File: jdelog.properties
 Processing File: settings.xml

------------------------------------------------------------------------
GENERATION SUCESSS
------------------------------------------------------------------------
 File: \tmp\build_jde_libs\JPS920\jdbj.ini generated
 File: \tmp\build_jde_libs\JPS920\jdeinterop.ini generated
 File: \tmp\build_jde_libs\JPS920\jdelog.properties generated
 File: \tmp\build_jde_libs\settings.xml generated
------------------------------------------------------------------------

```

It will create the following files:

```
build_jde_libs
      ?????? settings.xml
      ?????? JPS920
      ???????????????????????? jdbj.ini
      ???????????????????????? jdeinterop.ini
      ???????????????????????? jdelog.properties            
```

Adding manually "tnsnames.ora" for Oracle RDBMS based installations only.

Edit **jdeinterop.ini** and change the following values

```bash
[DNS_SERVERS]
JDE-ENT=2.2.2.2
JDE-SQL=1.1.1.1
```

The process Generate Ini Files will include both Logic Server and Database Server. You will need to set the correct ip's.

## JD Generate Jars Files

This tool will generate all jars files need it by JD Web Service Microservice.

![Configuration File Tool Image](images/JarTool.png "Configuration File Tool")

#### Preparing folders

Create folder with the following structure under **/tmp/build_jde_libs**:

```
tmp
??????build_jde_libs
      ?????? JDBC_Vendor_Drivers
      ?????? system
         ?????? Classes 
         ?????? JAS    
         ?????? WS
 
```

Copy files from JDE Deploment Server to the corresponding folders:
  
        
| Destination                              | Source                                             |
| ---------------------------------------- | -------------------------------------------------- |
|/tmp  |                    |
|/tmp/build_jde_libs        |                             |
|   ->/JDBC_Vendor_Drivers  | //Deployment Server/E920/MISC/*                    |
|   ->/system/Classes      | //Deployment Server/E920/system/Classes\*          |
|   ->/system/JAS          | //Deployment Server/E920/system/JAS/webclient.ear/webclient.war/WEB-INF/lib/*|
|   ->/system/WS           | //Deployment Server/E920\DV920/java/sbfjars/*      |

 
#### How define local Repository (localRepo option). 

This option is required to run this tool using JAVA apps locally

Running this command you can get where the curret local repository is defined:

```bash
mvn help:evaluate -Dexpression=settings.localRepository
```

```bash
[INFO]
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-help-plugin:3.2.0:evaluate (default-cli) @ standalone-pom ---
[INFO] No artifact parameter specified, using 'org.apache.maven:standalone-pom:pom:1' as project.
[INFO]
/root/.m2/repository  <==  ****** THIS IS THE VALUE REQUIRED *******
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.301 s
[INFO] Finished at: 2021-11-22T20:31:42Z
[INFO] ------------------------------------------------------------------------
```

In this output we see **/root/.m2/repository**

### Using JAVA APP

Download [JD Generate Jar Files]( http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-create-jar-files/1.0.0/jd-create-jar-files-1.0.0-jar-with-dependencies.jar "Generator") - latest release: 

```
curl http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-create-jar-files/1.0.0/jd-create-jar-files-1.0.0-jar-with-dependencies.jar --output jd-create-jar-files-1.0.0-jar-with-dependencies.jar
```

#### Create Jars File

```bash
Usage: java -jar jd-create-jar-files-1.0.0-jar-with-dependencies.jar OPTIONS

Options category 'startup':
  --accion [-a] (a string; default: "3")
    Accion: 1: Generate JDE Bundle 2: Build WS 3: Both
  --clean [-c] (a string; default: "Y")
    clean
  --jdbcDriver [-j] (a string; default: "/tmp/build_jde_libs/JDBC_Vendor_Drivers")
    Enter JDBC Driver Folder
  --jdeInstallPath [-i] (a string; default: "/tmp/build_jde_libs/")
    Enter JDE Path installed
  --localRepo [-r] (a string; default: "")
    Enter Maven Local Repo
  --settings [-s] (a string; default: "/tmp/build_jde_libs/settings.xml")
    settings.xml to use Ex. /apache-maven-3.8.1/conf/settings.xml
  --version [-o] (a string; default: "1.0.0")
    Enter Version
```

#### Usage Generate Jars Files Exampes

Run following command:
 
```bash
java -jar jd-create-jar-files-1.0.0-jar-with-dependencies.jar -r /root/.m2/repository
```

### Using Docker

```bash
docker run --rm -v /tmp/build_jde_libs:/tmp/build_jde_libs/ -i --name jd-create-jar-files 92455890/jd-create-jar-files:1.0.0
```

### Output 

```bash
------------------------------------------------------------------------
GENERATION SUCESSS
------------------------------------------------------------------------
JDE Library bundle has been copied to: /tmp/build_jde_libs/jde-lib-wrapped-1.0.0.jar
JDE WS has been copied to: /tmp/build_jde_libs/StdWebService-1.0.0.jar
------------------------------------------------------------------------
```

It will create the following files:

```
build_jde_libs
      ?????? jde-lib-wrapped-1.0.0.jar
      ?????? StdWebService-1.0.0.jar

```

#### Deploy artifact to Local Repository - Optional

This optional. 
At startup, JD Microservice has the option to get these libraries from an internal repository.
 

In case you want to use a local repository, exectute the following command:

```
mvn deploy:deploy-file -DgroupId=com.jdedwards -DartifactId=jde-lib-wrapped -Dversion=1.0.0 -DrepositoryId=repo-central -Dpackaging=jar -Dfile=\tmp\build_jde_libs\jde-lib-wrapped-1.0.0.jar -Durl=http://localrepo:8081/artifactory/libs-release
mvn deploy:deploy-file -DgroupId=com.jdedwards -DartifactId=StdWebService -Dversion=1.0.0 -DrepositoryId=repo-central -Dpackaging=jar -Dfile=\tmp\build_jde_libs\StdWebService-1.0.0.jar -Durl=http://localrepo:8081/artifactory/libs-release
```




## Start JD Web Services Microservice / JDE Rest API Microservice

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

#### DNS defintion

Use [DNS_SERVERS] section inside jdeinterop.ini files to complete theses values:

```bash
#
# ====================================
# ETC/HOST
# ====================================
#
JDE_MICROSERVER_ENTERPRISE_SERVER_NAME=JDE-ENT
JDE_MICROSERVER_ENTERPRISE_SERVER_IP=1.1.1.1
JDE_MICROSERVER_ENTERPRISE_DB_NAME=JDE-DATABASE 
JDE_MICROSERVER_ENTERPRISE_DB_IP=2.2.2.2
#
```

### Create and Run JD Microservice

Run following docker commands:

Create Container

```bash
docker-compose -f docker-compose-dist.yml up --no-start
```

Copy files Configuration files and jars files into Container

```bash
docker cp /tmp/build_jde_libs/JPS920 jd-atina-microserver:/tmp/jde/config
docker cp /tmp/build_jde_libs/jde-lib-wrapped-1.0.0.jar jd-atina-microserver:/tmp/jde
docker cp /tmp/build_jde_libs/StdWebService-1.0.0.jar jd-atina-microserver:/tmp/jde
```

Run Containers

```bash
docker-compose -f docker-compose-dist.yml start
```

### Check JD WS Microservice startup

Check starting process for JD Atina WS Microservice

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

At the end of the logs, you will the log that you need to check. Run the following docker command:

```bash
docker exec -it jd-atina-microserver cat /tmp/jde/JDEConnectorServerLog/jde_atina_server_2021-11-18.0.log
```

```bash
19:27:06.713 [main] INFO  c.acqua.jde.jdeconnectorserver.JDEConnectorServer - Iniciando JDE Service Impl...
19:27:06.891 [main] INFO  c.a.jde.jdeconnectorserver.server.JDERestServer - *-------------------------------------*
19:27:06.892 [main] INFO  c.a.jde.jdeconnectorserver.server.JDERestServer - *   Starting JDE Microservice 1.0.0   *
19:27:07.061 [main] INFO  c.a.jde.jdeconnectorserver.server.JDERestServer - *   JDE Microservice started!         *
19:27:07.064 [main] INFO  c.a.jde.jdeconnectorserver.server.JDERestServer - *-------------------------------------*
```

The other way to see the logs is the following command:

```bash
docker logs jd-atina-microserver
```

### Check JD Rest API Microservice startup

```bash
docker logs jd-atina-rest-server
```

Output

```bash
exec java -Dquarkus.http.host=0.0.0.0 -Djava.util.logging.manager=org.jboss.logmanager.LogManager -XX:+ExitOnOutOfMemoryError -cp . -jar /deployments/quarkus-run.jar
           ,ggg,
          dP""8I    I8
         dP   88    I8
        dP    88 88888888 gg
       ,8'    88    I8    ""
       d88888888    I8    gg    ,ggg,,ggg,     ,gggg,gg
 __   ,8"     88    I8    88   ,8" "8P" "8,   dP"  "Y8I
dP"  ,8P      Y8   ,I8,   88   I8   8I   8I  i8'    ,8I
Yb,_,dP       `8b,,d88b,_,88,_,dP   8I   Yb,,d8,   ,d8b,
 "Y8P"         `Y88P""Y88P""Y88P'   8I   `Y8P"Y8888P"`Y8
                          Powered by Quarkus 2.5.0.Final
19:32:14 INFO  [io.quarkus] (main) jd-atina-rest-server 1.0.0 on JVM (powered by Quarkus 2.5.0.Final) started in 8.869s. Listening on: http://0.0.0.0:8081 and https://0.0.0.0:8082
19:32:14 INFO  [io.quarkus] (main) Profile prod activated.
19:32:14 INFO  [io.quarkus] (main) Installed features: [cdi, logging-gelf, qute, resteasy, resteasy-jackson, resteasy-qute, scheduler, smallrye-context-propagation, smallrye-openapi, swagger-ui, vertx]
```

Go to API defintions:

[https://localhost/swagger/]( https://localhost/swagger/ "https://localhost/swagger/")



 
## JDE Check JD Microservice executing a Web Service
 
This tool is used to test JD Microservice. 
It will invoke a simple WS.

### Installation

Download [JD Check JD Microservice]( http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-check-microservice/1.0.0/jd-check-microservice-1.0.0-jar-with-dependencies.jar "Generator") - latest release: 

```
curl http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-check-microservice/1.0.0/jd-check-microservice-1.0.0-jar-with-dependencies.jar --output jd-check-microservice-1.0.0-jar-with-dependencies.jar
```
### Run JD Check Microservice
 
```bash
Usage: java -jar jd-check-microservice OPTIONS

   Options category 'startup':

  --mode [-m] (a string; default: "TestLoggindAndGetAddressBookWS")
    Modes
  --user [-u] (a string; default: "")
    JDE User
  --password [-w] (a string; default: "")
    JDE Password
  --environment [-e] (a string; default: "")
    JDE Environment
  --role [-r] (a string; default: "")
    JDE Role
  --serverName [-s] (a string; default: "")
    JD Micreserver Name or IP
  --serverPort [-p] (a string; default: "")
    JD Micreserver Port
  --addressbookno [-a] (a string; default: "")
    Address Book No

```
Usage Exampes

```bash
java -jar jd-check-microservice-1.0.0-jar-with-dependencies.jar -u JDE -w XXXXXX -e JDV920 -r *ALL -s localhost -p 8077 -m TestLoggindAndGetAddressBookWS -a 28
```

Output 

```bash
Checking Microservice...
user=JDE, environment=JDV920, role=*ALL, serverName=192.168.99.100, serverPort=8077, mode=TestLoggindAndGetAddressBookWS, addressbookno=1, sessionId=, transactionId=
Transaction ID: 20211122124518
User User: JDE in environment JDV920 with *ALL connected with Session ID [-1636010069]
WS JP010000.AddressBookManager.getAddressBook has been called correctly
Logout [0]
------------------------------------------------------------------------
CHECK SUCESSS
------------------------------------------------------------------------
User User: JDE in environment JDV920 with *ALL connected with Session ID -1636010069
Address Book Name: Financial/Distribution Company --
User User: JDE in environment JDV920 with *ALL disconnected. Current session ID 0
------------------------------------------------------------------------
``` 
## Install Mulesoft JDE WS CE Connector Mule 3

The Atina JDE Web Service Connector CE provides interoperability with Oracle???s JD Edwards EnterpriseOne??? it allow consume all JDE Web Services published

### Installation

Download [ Atina JDE Web Service Connector CE]( http://157.245.236.175:8081/artifactory/libs-release/com/atina/jd-atina-connector/1.0.0/jd-atina-connector-1.0.0.zip "Generator") - latest release: 

```
curl  http://157.245.236.175:8081/artifactory/libs-release/com/atina/jd-atina-connector/1.0.0/jd-atina-connector-1.0.0.zip -o jd-atina-connector-1.0.0.zip
```

Install From the Studio Help Menu:

* Click Help > Install New Software.

* Click Add > Then, click Archive.. and select jd-atina-connector-1.0.0.zip downloaded

* Expand a Community Category and click the checkbox for the connector to install.

* Click Next, Next, I accept the terms of the license agreement, Finish, and restart Studio when prompted.

* From a Mule Project in Studio, search for the connector and drag it to the Canvas.








