
# JD Web Service Microservice

This tool allow get all information need it from JDE Enterprise Server Manager to generate ini files used by JD Web Service Microservice.




## Prerequisites

 - JDE credential for HTML Client. User, Pasword, Environment and Role.

 - JAVA OpenJDK

 - Oracle JD Edwards EnterpriseOne Server Manager credential. Ex. http://server:8999/manage/

 - [Docker](  https://docs.docker.com/get-docker/ "Docker"). JDE Web Service Microserver run under Docker container.

 
## Installation

Download [JD Generate Ini Files]( http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-generate-ini-files/1.0.0/jd-generate-ini-files-1.0.0-runner.jar "Generator") - latest release: 

```
curl http://157.245.236.175:8081/artifactory/libs-release-local/com/atina/jd-generate-ini-files/1.0.0/jd-generate-ini-files-1.0.0-runner.jar --output jd-generate-ini-files-1.0.0-runner.jar
```



    
## Uses

### Run JD Generate Ini Files

This tool allow get all information need it from JDE Enterprise Server Manager to generate ini files used by JD Web Service Microservice.
 
```bash
  java -jar jd-generate-ini-files-1.0.0-runner.jar [OPTIONS]

  OPTIONS:                                                                       
  -u, --user[=<user>]         JDE User for Enterprise Server Manager
  -p, --password[=<password>] JDE Password for Enterprise Server Manager
  -s, --server[=<server>]     JDE URL of Server Manager

```
Usage Exampes

```bash
java -jar jd-generate-ini-files-1.0.0-runner.jar -u jde_server_manager_user -p password_server_manager -s http://server-manager:8999/manage
```
 
