# How to setup SonarQube with Jenkins to hide the URL and key

* Manage Plugins in Jenkins (should put into our template probably)
* Go to "Available" and filter on "sonarqube"
* Add the SonarQube Scanner version 2.8.1 or higher at (http://updates.jenkins-ci.org/download/plugins/sonar/2.8.1/sonar.hpi) but currently does not work from Jenkins on OCP NAVAIR!) so I had to d/l and add under "advanced" in manage plugins
* Install without restart
* Configure under the Manage Jenkins --> Configure System and come down to the SonarQube Servers. Check the 'enable environment variables' and add a server setup.

This should let us do something like  `dotnet sonarscanner begin /k:datasvc-contacts /d:sonar.host.url=$SONAR_HOST_URL /d:sonar.login=$SONAR_AUTH_TOKEN`  and to end it with  `dotnet sonarscanner end /d:sonar.login=$SONAR_AUTH_TOKEN`  for the dotnet core global tool one we use. Or with the MSBuild.exe one as well with similar tokens. the  `$`  is for Linux. We would wrap front and back with  `%`  if on Windows :face_vomiting:.  Needs to be tested and then documented so people use it like that. Having the Jenkins laid out w/ these and the jenkins slave setup will help for sure.  We may want to use with Quality Gates as well to improve upon them. I have that in there but I kill it after 15 minutes I believe. We ought to test this so we keep keys out of the repo.

My example: https://bitbucket.di2e.net/projects/AD722ET/repos/datasvc-contacts/browse/Jenkinsfile

Read More:
https://jenkins.io/doc/pipeline/steps/sonar/#waitforqualitygate-wait-for-sonarqube-analysis-to-be-completed-and-return-quality-gate-status
https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Jenkins (edited) 

We are on SQ 6.7

## Example of this

```
    stage('Scan Codebase') {
      agent { label 'sonar-dotnet' }
      steps {
        withSonarQubeEnv('NAVAIR-SonarQube-Server') {
          sh "dotnet sonarscanner begin /k:datasvc-contacts /d:sonar.host.url=$SONAR_HOST_URL /d:sonar.login=$SONAR_AUTH_TOKEN"
          sh "dotnet build contacts"
          sh "dotnet sonarscanner end /d:sonar.login=$SONAR_AUTH_TOKEN"
        }
      }
    }
```