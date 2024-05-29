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

options {
  buildDiscarder logRotator(artifactDaysToKeepStr: '2', artifactNumToKeepStr: '2', daysToKeepStr: '3', numToKeepStr: '5')
}


    
    stages{


        stage('CheckOut'){
            jenkins_slack_channel('STARTED')
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
        
        /*
        stage('CodeQualityCheck'){
            steps{
                sh "mvn clean sonar:sonar package"
            }
        }

        stage('Nexus'){
            steps{
                sh "mvn clean deploy"
            }
        }
        */

        

        stage('AWS_ECR_AUTHENTICATE'){
            steps{
                sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com"
                sh "docker build -t ${AWS_ECR_REPO}:${BUILD_NUMBER} ."
            }
        }

        stage('PushtoECR'){
            steps{
                sh "docker tag ${AWS_ECR_REPO}:${BUILD_NUMBER} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${AWS_ECR_REPO}:${BUILD_NUMBER}"
                sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${AWS_ECR_REPO}:${BUILD_NUMBER}"
            }
        }

        
        stage('Verify kubectl') {
            steps {
                sh 'kubectl version --client'
        }
    }
    /*
        stage('K8s Node'){
            steps{
                sh "kubectl get nodes"
        }
    }

        stage('Deploy to EKS cluster'){
            steps{
                sh "helm upgrade --install first helmchart --namespace test-ns --set image.tag=$BUILD_NUMBER"
                
                
            }
        }

        */
    
        


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
}

def sendslacknotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  //This is the condition which we are checking weather buildStatus is SUCCESSFULL or not.
 //This line updated to show the Eclipse with GitHub demo
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    colorName = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    colorName = 'GREEN'
    colorCode = '#00FF00'
  } else {
    colorName = 'RED'
    colorCode = '#FF0000'
  }

  // Calling the slackSend function to Send notifications.
  slackSend (color: colorCode, message: summary,channel: '#jenkins_slack_channel')
}


		
		
		