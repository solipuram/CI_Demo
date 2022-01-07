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
                sh 'mvn clean deploy -P release'
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
