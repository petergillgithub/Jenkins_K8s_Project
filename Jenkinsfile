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
        
        // stage('CodeQualityCheck'){
        //     steps{
        //         sh "mvn clean sonar:sonar package"
        //     }
        // }

        // stage('Nexus'){
        //     steps{
        //         sh "mvn clean deploy"
        //     }
        // }

        

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

    stage('Verify helm version') {
    steps {
        sh 'helm version'
    }
}

        stage('Deploy to EKS cluster'){
            steps{
                sh "helm upgrade --install javawebapp ./javawebapp \ --set image.repository=${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${AWS_ECR_REPO} --set image.tag=${BUILD_NUMBER}"
            }
        }


        









    }
}