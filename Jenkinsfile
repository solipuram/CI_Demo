pipeline {
    agent any

    stages 
    {
        stage('Git clone') 
        {
            steps 
            {
                  git url: 'https://github.com/solipuram/webapp.git', credentialsId: 'cidemo',branch: 'master'
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
