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
## Goal Name
mvn clean deploy -P release


## Add Maven Config file in Jenkins (This step is for when Jenkins setup is in Master & Slave)
Go to Jenkins -> Select respective team master and folder -> Select Config files option at folder level ->Add a new config file -> Select Maven settings file  

```
• ID - <Team-Name>NexusGlobalSettings
• Name - NexusSettings
• Comment - <Team-Name> Nexus maven settings.xml file
• Replace All - uncheck
• Add Server Credentials
• Server Id - nexus
• Credentials - select the credential <Team-name>-Nexus-CI-User.
• Add below file content.
```
