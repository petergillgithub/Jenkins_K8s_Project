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

    



    }
}