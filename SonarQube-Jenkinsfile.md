# How to setup SonarQube with Jenkins to hide the URL and key

* Manage Plugins in Jenkins (should put into our template probably)
* Go to "Available" and filter on "sonarqube"
* Add the SonarQube Scanner version 2.8.1 or higher at (http://updates.jenkins-ci.org/download/plugins/sonar/2.8.1/sonar.hpi) but currently does not work from Jenkins on OCP NAVAIR!) so I had to d/l and add under "advanced" in manage plugins
* Install without restart (or build into the image you use!)
* Configure under the Manage Jenkins --> Configure System and come down to the SonarQube Servers. Check the 'enable environment variables' and add a server setup.

This should let us do something like 'dotnet sonarscanner begin /k:project-name /d:sonar.host.url=$SONAR_HOST_URL /d:sonar.login=$SONAR_AUTH_TOKEN' and to end it with 'dotnet sonarscanner end /d:sonar.login=$SONAR_AUTH_TOKEN' for the dotnet core global tool. 

We are on SQ 7.4 community edition

## Example of this

```
    stage('Scan Codebase') {
      agent { label 'sonar-dotnet' }
      steps {
        withSonarQubeEnv('NAVAIR-SonarQube-Server') {
          sh "dotnet sonarscanner begin /k:projectname /d:sonar.host.url=$SONAR_HOST_URL /d:sonar.login=$SONAR_AUTH_TOKEN"
          sh "dotnet build projectname"
          sh "dotnet sonarscanner end /d:sonar.login=$SONAR_AUTH_TOKEN"
        }
      }
    }
```
