pipeline{


agent any

tools {
  maven 'maven3.9.6'
}

environment{
    AWS_ACCOUNT_ID = "339712876743"
    AWS_DEFAULT_REGION = "eu-west-2"
    AWS_ECR_REPO = "ecrrepo" 
    // DOCKER_IMAGE = ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${AWS_ECR_REPO}:${BUILD_NUMBER}
    



}

stages{

    // CheckOutCode
    stage('CheckOutCode'){
        steps{
            checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/petergillgithub/maven-web-application.git']])        }
    }

    // Build the package with maven
    stage('BuildPackage'){
        steps{
            sh "mvn clean package -DskipTests"
        }
    }

    // Unit Testing
    stage('Unit Tests') {
        steps {
            sh 'mvn test'
        }

    //SonarQube
    stage('Sonar_Qube_Code_QualityCheck'){
        steps{
            sh "docker "
        }
    }



    stage('Logging into AWS ECR'){
        steps{
            sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
        }

    }
    stage('BuildDockerImage'){
        steps{
            sh "docker build -t ${AWS_ECR_REPO}:${BUILD_NUMBER} ."
        }
    }

    stage('Push to ECR Repo'){
        steps{
            sh "docker tag ${AWS_ECR_REPO}:${BUILD_NUMBER} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${AWS_ECR_REPO}:${BUILD_NUMBER}"
            sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${AWS_ECR_REPO}:${BUILD_NUMBER}"
        }
    }

    stage('DeploytoK8s'){
        steps{
            sh "kubectl apply -f javawebapp-deployment.yml"
        }
    }







    }
}


// #################################################################

pipeline {


agent any

tools {
  maven 'maven3.9.6'
}

/*
environment{
    AWS_ACCOUNT_ID = "339712876743"
    AWS_DEFAULT_REGION = "eu-west-2"
    AWS_ECR_REPO = "ecrrepo" 
    // DOCKER_IMAGE = ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${AWS_ECR_REPO}:${BUILD_NUMBER}

}

*/

triggers {
  pollSCM '* * * * *'
}

stages{

    // CheckOutCode
    stage('CheckOutCode'){
        steps{
            checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/petergillgithub/maven-web-application.git']])        }
    }

    // Build the package with maven
    stage('BuildPackage'){
        steps{
            sh "mvn clean package -DskipTests"
        }
    }
    
    // Unit Testing
    stage('Unit Tests') {
        steps {
            sh 'mvn test'
        }
    // SonarQube
    stage('SonarQubeCode_quality'){
        steps{
            sh " docker build -t sonarqube:alpine"
        }
    }
    
    
    
    
    } 

} // stages closing

} // pipeline closing