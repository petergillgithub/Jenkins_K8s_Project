// Shared Library for Slack
@Library('slack_library') _

pipeline {


agent any 
    
tools {
maven 'maven3.9.7'
}

environment {
    AWS_ACCOUNT_ID = "339712876743"
    AWS_ECR_REPO = "ecrrepo"
    AWS_REGION = "eu-west-2"

}

triggers {
  pollSCM '* * * * *'
}


options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '1')
  timestamps()
}

    
stages{
// CheckOut the Code from Git 
        stage('CheckOut'){
            steps{
                sendSlackNotifications('STARTED')
                checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/petergillgithub/Jenkins_K8s_Project.git']])
            }
        }
// Build the package
        stage('Compile the package'){
            steps{
                sh "mvn clean package"
            }
        }
// Unit Testing
        stage('Unit Test'){
            steps{
                sh "mvn test"
            }
        }
        
// Code Quality Check from Sonar Qube
        stage('CodeQualityCheck'){
            steps{
                sh "mvn clean sonar:sonar package"
            }
        }
// Push the Dependencies to Nexus Repo
        stage('Nexus'){
            steps{
                sh "mvn clean deploy"
            }
        }
// AWS ECR Authentication & Build the package in container with Docker
        stage('AWS_ECR_AUTHENTICATE'){
            steps{
                sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                sh "docker build -t ${AWS_ECR_REPO}:${BUILD_NUMBER} ."
            }
        }
// Push the Image to Aws ECR & tag the image
        stage('PushtoECR'){
            steps{
                sh "docker tag ${AWS_ECR_REPO}:${BUILD_NUMBER} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${AWS_ECR_REPO}:${BUILD_NUMBER}"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${AWS_ECR_REPO}:${BUILD_NUMBER}"
            }
        }
// Verfiy Kubernetes version
        stage('Verify kubectl') {
            steps {
                sh 'kubectl version --client'
        }
    }
// Check Nodes of Kubernetes Cluster
        stage('K8s Node'){
            steps{
                sh "kubectl get nodes"
        }
    }
// Deploy the application to EKS cluster
        stage('Deploy to EKS cluster'){
            steps{
                sh "helm upgrade --install first helmchart --namespace test-ns --set image.tag=$BUILD_NUMBER"
                
                
            }
        }

        
// Verify Job Name & Build Number, Node Name, Jenkins Home Dir with env var
        stage("Environment variables"){
            steps{
                script{
                // echo "The Job Name is : ${env.JOB_NAME}"
                // echo "The Build Number is:  ${env.BUILD_NUMBER}"
                // echo "The Node Name is:  ${env.NODE_NAME}"
                // echo "The Jenkins Home Directory is:  ${env.JENKINS_HOME}"
                def jobName = env.JOB_NAME
                def buildNumber = env.BUILD_NUMBER
                def nodeName = env.NODE_NAME
                def jenkinsHome = env.JENKINS_HOME
                def combinedMessage = "The Job Name is: ${env.JOB_NAME}\nThe Build Number is: ${env.BUILD_NUMBER}\nThe Node name is: ${env.NODE_NAME}\nThe Jenkins Home Directory is: ${env.JENKINS_HOME}"
                echo combinedMessage
                }

            }
        }


    }
// Slack Notification part to notify slack of either success or failure.
    post {
  success {
    sendSlackNotifications(currentBuild.result)
  }
  failure {
    sendSlackNotifications(currentBuild.result)

  }
}

}




		
		
		
