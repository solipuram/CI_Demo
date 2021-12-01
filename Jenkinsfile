pipeline {
    agent any

    stages 
    {
        stage('Git clone') 
        {
            steps 
            {
                git credentialsId: '5fb13f39-c47f-45e1-ba70-88be1a6df992', url: 'https://github.com/solipuram/webapp.git'
            }
        }
        stage('Compile') 
        {
            steps 
            {
                sh 'mvn clean install'
            }
        }
        
        
             
        
        
        
    }
}
