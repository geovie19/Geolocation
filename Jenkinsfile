pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '017589840041.dkr.ecr.us-east-1.amazonaws.com/my-ecr'						

    registryCredential = 'jenkins'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/geovie19/Geolocation.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Pushing to ECR') {
            steps{
                script {
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 017589840041.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 017589840041.dkr.ecr.us-east-1.amazonaws.com/my-ecr'
                }
            }
        }
    }
}  
    
