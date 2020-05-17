# sonarqube-start-in-10min

## Intro

This is instruction how to set up playground to learn SonarQube.
**This setup should not used as production setup!** (and believe me there are a lot of reasons for that! (no volumes, sonar latest community versions has bugs, this setup runs embedded db - which is being destroid on every run etc.))

### Related links

<https://www.sonarqube.org/>  
SonarQube docker site: <https://hub.docker.com/_/sonarqube/>

## SonarQube server

## Run SonarQube server

**!Remember** attach volumes etc. if you need persistence of data

```bash
git pull sonarqube
docker run -d --stop-timeout 3600 -p 9000:9000 -p 9092:9092
```

> **Note!** You can try running owasp/sonarqube setup on another port! <https://github.com/OWASP/sonarqube> and compare results.
> OWASP implemenations contains several plugins relted to security

## Create Sonar Project

### Create project

`curl -X POST -u admin:admin "http://localhost:9000/api/projects/create?name=SonarPlay&project=SonarPlay&visibility=public"`
<http://localhost:9000/dashboard?id=SonarPlay>  

or  

Open Project -> Create project -> SonarPlay/SonarPlay  

### Generate project key

Generate Key: SonarPlay -> `<project key>` (replace references below)
You can setup target languge.

## Clone project and create Config files

```bash
cd /home/karlis/docker/data
git clone https://github.com/laravel/laravel
cd laravel
echo 'sonar.projectKey=SonarPlay' > sonar-project.properties
echo 'sonar.sources=app' >> sonar-project.properties
echo 'sonar.host.url=http://localhost:9000' >> sonar-project.properties
# Remember to set project key
echo 'sonar.login=<project key>' >> sonar-project.properties
echo 'sonar.tests=tests' >> sonar-project.properties
echo 'sonar.sourceEncoding=UTF-8' >> sonar-project.properties
# On my version setup require both files sonar-scanner & sconar-project .properties to execute
cat sonar-project.properties > sonar-scanner.properties



## SonarQube scanner

```bash
docker pull sonarsource/sonar-project-cli
docker run --rm --network=host -e SONAR_HOST_URL=http://localhost:9000 -it -v "/home/karlis/docker/data/laravel:/usr/src" sonarsource/sonar-scanner-cli
```

> **Note!** there is warning in log which can be ignored <https://community.sonarsource.com/t/sonar-scanner-throws-warnings-and-error-since-last-week/11821/3>

## Misc

`http://localhost:9000/api/webservices/list`


