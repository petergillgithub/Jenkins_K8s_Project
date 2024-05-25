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
                sh "mvn sonar:sonar \
  -Dsonar.host.url=http://ec2-3-11-80-220.eu-west-2.compute.amazonaws.com:9000 \
  -Dsonar.login=d157f55828d6ddc14427965b5d7d56b6a76e4cda"
                sh "mvn clean sonar:sonar"
            }
        }






    }
}