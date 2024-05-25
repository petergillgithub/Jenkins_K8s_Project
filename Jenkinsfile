pipeline {

agent any 
    
tools {
maven 'maven3.9.6'
}

    
    stages{

        stage('CheckOut'){
            steps{
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/petergillgithub/Jenkins_K8s_Project.git']])
            }
        }

        stage('Compile the package'){
            steps{
                sh "mvn clean package"
            }
        }

        stage('Unit Test'){
            steps{
                sh "mvn test"
            }
        }

        stage('CodeQualityCheck'){
            steps{
                
                sh "mvn clean install sonar:sonar"
            }
        }






    }
}