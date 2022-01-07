pipeline {
    agent any

    stages 
    {
        stage('Git clone') 
        {
            steps 
            {
                  git url: 'https://github.com/solipuram/CI_Demo.git', credentialsId: 'cidemo',branch: 'master'
            }
        }
        stage('Compile') 
        {
            steps 
            {
                sh 'mvn clean install'
            }
        }
        
        stage('Publish to Nexus')
        {
             steps
            {
                //sh 'mvn clean deploy'
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: 'https://192.168.1.16:8081/repository/reddy-dev/',
                    groupId: 'reddy-dev',
                    version: '1.0.0',
                    repository: 'ci-demo',
                    credentialsId: 'CredentialsId',
                    artifacts: [
                        [artifactId: 'ci-demo',
                        classifier: '',
                        file: 'target/ci-demo' + version + '.jar',
                        type: 'jar']])
 
            }
        }
         stage('Deploy on tomcat')
        {
             steps
            {
                //sh 'mvn tomcat7:deploy'
                echo "Deploying on tomcat"
            }
        }
             
        
        
        
    }
}
