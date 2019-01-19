# OpenShift and MiniShift templates, Dockerfiles, slave images for Jenkins

## OpenShift Templates
* sonarqube-postgresql-template.yaml for SonarQube 7.5 Community Edition and PostgreSQL with saved volumes for plugins
* keycloak-in-openshift for Keycloak setup

## Dockerfiles for different things
* using NodeJS 8+ and the oracle driver node-oracledb

## Jenkins slave images for the Jenkins w/in OpenShift/Minishift
* Jenkins slave dotnet core sdk with the global sonarscanner tool installed
* Jenkins slave with regular sonar-scanner installed for NodeJS and other non-dotnet-core items

## Jenkinsfile examples
* Jenkinsfile.dotnetcore for .NET Core building
* Jenkinsfile.nodejs for NodeJS 8