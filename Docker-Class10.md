### Multi Stage Docker builds
* To build a docker image we need an application package, we build a application package on the build server and then use it in the docker file to build the application docker image.
* The idea of multi stage builds is to use docker completely to build an application package and create a docker image.
* Lets try to build the docker image for game of life [Refer Here](https://github.com/wakaleo/game-of-life) for the code of game of life
* To build this application package we need
    * git to the code
    * java to be installed
    * maven to build the code
* Manual Steps to get the application package:

```
git clone https://github.com/wakaleo/game-of-life.git
cd game-of-life
mvn package
```
* Now after the package step is successful we get the gameoflife.war which can be used in the Dockerfile to build image
* We have a docker image which is preinstalled with maven [Refer Here](https://hub.docker.com/_/maven)

```
FROM maven:3-jdk-8 AS mvn
RUN git clone https://github.com/wakaleo/game-of-life.git && cd game-of-life && mvn package


FROM tomcat:8
LABEL author="Josef"
LABEL organization="DevopsEasy"
COPY --from=mvn /game-of-life/gameoflife-web/target/gameoflife.war /usr/local/tomcat/webapps/gameoflife.war
EXPOSE 8080
```
* Now build the Dockerfile

```
docker image build -t gameoflife:1.0 .
```
![Preview](./Images/docker121.png)

* Docker first tries to build the stages in the above case it will build the mvn stage first and then it builds the docker image with the last FROM which will be what your image will have. the necessary files will be copied from stages into the Docker image depending on instructions

* Lets run the container

![Preview](./Images/docker122.png)

![Preview](./Images/docker123.png)

* Now lets try to build one more docker image using the multi staged build

* Spring pet clinic

* Manual steps for build the package:

```
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
mvn package
```
* Dockerfile Provided below

```
FROM maven:3-jdk-11 as mvn
RUN git clone https://github.com/spring-projects/spring-petclinic.git 
RUN cd spring-petclinic && mvn package


FROM openjdk:11
LABEL author="Josef"
COPY --from=mvn /spring-petclinic/target/spring-petclinic-2.4.2.jar /spring-petclinic-2.4.2.jar
EXPOSE 8080
CMD ["java", "-jar", "/spring-petclinic-2.4.2.jar"]
```
* Now build the Dockerfile

```
docker image build -t spc:1.0 .
```

![Preview](./Images/docker124.png)

* Now lets run the container

![Preview](./Images/docker125.png)

![Preview](./Images/docker126.png)

### Docker on Windows Server
* Lets create a Windows Server 2016 instance in any cloud
* Launch powershell as admin and execute the following commands

```
Set-ExecutionPolicy unrestricted
 [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
Install-Module -Name DockerMsftProvider -Repository PSGallery -Force
Install-Package -Name docker -ProviderName DockerMsftProvider
Restart-Computer -Force
```

![Preview](./Images/docker128.png)

* Post machine restart, Launch Powershell as admin

```
docker info
```

![Preview](./Images/docker129.png)

* Now lets try to pull iis server image

```
docker pull mcr.microsoft.com/windows/servercore/iis
docker image ls
```
* Now lets run the container

```
docker container run -d -P mcr.microsoft.com/windows/servercore/iis:latest
docker container ls
```
* Check container is runningor not by pasting public ip in browser
* Run the following command

```
docker container exec -it <container id/name> powershell

```
![Preview](./Images/docker135.png)

![Preview](./Images/docker136.png)


* To get info about the disks in the container run the powershell commands as [Refer Here](https://www.ghacks.net/2019/05/28/look-up-hard-disk-information-with-powershell/)



