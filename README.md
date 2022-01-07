# CI_Demo

This is Sample Continuous Integration Workflow Demo .In this example have taken a sample java code.

There are four stages in this CI_Demo

	• Git Checkout 
	• MVN Build
	• Publish to Nexus
	• Deploy on Tomcat

## GIT Checkout:

Initially configure the credentials in the Jenkins -->Manage Jenkins-->Global Credentials.

| UserName      | Github Email |
| ---           | ---       |
| Password      | Github pwd |
| Id            | Cidemo|
| Descrption    | Ci_workflow_demo|

//the below command checkout the git code.

git url: 'https://github.com/solipuram/CI_Demo.git', credentialsId: 'cidemo',branch: 'master'

## Build.

In this example have used maven as build tool.

Goal Name as mvn build install.

## Publish to Nexus.

## Goal Name
mvn clean deploy 

Add the distribution managemnet in the pom.xml file

Example:
```
  <distributionManagement>
    <repository>
      <id>nexus</id>
      <name>Releases</name>
      <url>http://192.168.1.16:8081/repository/maven-releases</url>
      </repository>
    <snapshotRepository>
      <id>nexus</id>
      <name>Snapshot</name>
      <url>http://192.168.1.16:8081/repository/maven-snapshots</url>
    </snapshotRepository>
  </distributionManagement>
```

##Add the Nexus Credinatls in setting.xml on the 
file in local machine at  /usr/share/maven/conf/settings.xml and add a server block to it.
```
 <servers>
    <server>
      <id>nexus</id>
      <username><nexus username></username>
      <password><nexus pwd></password>
    </server>
 </servers>
```



## The below steps for when Jenkins is running Master & Slave

Add the distribution managemnet in the pom.xml file

Example:
```
  <distributionManagement>
    <repository>
      <id>nexus</id>
      <name>Releases</name>
      <url>http://192.168.1.16:8081/repository/maven-releases</url>
      </repository>
    <snapshotRepository>
      <id>nexus</id>
      <name>Snapshot</name>
      <url>http://192.168.1.16:8081/repository/maven-snapshots</url>
    </snapshotRepository>
  </distributionManagement>
```


###### Config file in Jenkins.

Go to Jenkins -> Select respective team master and folder -> Select Config files option at folder level ->Add a new config file -> Select Maven settings file  

```
• ID -
• Name - NexusSettings
• Comment - Nexus_maven_settings.xml file
• Replace All - uncheck
• Add Server Credentials
• Server Id - nexus
• Credentials - select the credential <Team-name>-Nexus-CI-User.
• Add below file content.

![Nexus_Settings](https://user-images.githubusercontent.com/3168102/148501304-7714dc1f-cb9e-44ba-8096-d685313271f1.PNG)

```

setting.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <mirrors>
    <mirror>

      <!--all requests to nexus via the mirror -->
      <!--This sends everything else to repository/<team-group-repo>/ -->
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>https://<nexus-ip>/repository/<team-group-repo>/</url>
      <url>https://192.168.1.16:8081/reposiroty/reddy-maven-group</url>
    </mirror>
  </mirrors>
  <servers>
  </servers>
  <profiles>
    <profile>
      <id>nexus</id>
      <properties>
        <service.repo.url>https://<nexus-ip>/repository/<team-release-repo>/</service.repo.url>
	<release.repo.url>https://<nexus-ip>/repository/<team-release-repo>/</release.repo.url>
	<snapshot.repo.url>https://<nexus-ip>/repository/<team-release-repo>/</snapshot.repo.url>
	
        <service.repo.url>https://192.168.1.16:808a/repository/reddy-services/</service.repo.url>
	<release.repo.url>https://192.168.1.16:8081/repository/<reddy-maven-release></release.repo.url>
	<snapshot.repo.url>https://192.168.1.16:8081/repository/<reddy-maven-snapshots>/</snapshot.repo.url>
      </properties>
      <!--Enable snapshots for the built in central repo to direct -->
      <repositories>
        <repository>
          <id>central</id>
          <url>http://central</url>
          <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
          </releases>
          <snapshots>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
          </snapshots>
        </repository>
      </repositories>
     <pluginRepositories>
        <pluginRepository>
          <id>central</id>
          <url>http://central</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </pluginRepository>
      </pluginRepositories>
    </profile>
  </profiles>
  <activeProfiles>
    <!--make the profile active all the time -->
    <activeProfile>nexus</activeProfile>
  </activeProfiles>
</settings>

```


### Publish to Nexus

```
stage('Publish artifacts to Nexus')
{
	container('maven') 
	{
		configFileProvider([configFile(fileId: '<Team-name>NexusSettings', variable: 'MAVEN_SETTINGS')])
		{
		     sh "mvn -s $MAVEN_SETTINGS deploy"
		 }
        }
}
```


###  Deploy on Tomcat

This stage is not completed
