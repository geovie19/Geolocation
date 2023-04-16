pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '676334272140.dkr.ecr.us-east-1.amazonaws.com/ernesto'						

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
        stage('upload war to Nexus') {
          steps {
            nexusArtifactUploader artifacts:[
                [
                    artifactId: 'bioMedical', 
                    classifier: '', 
                    file: 'target/bioMedical-2.2.4',
                    type: 'image'
                ]
            ],
            credentialsId: 'nexus2', 
            groupId: 'org.springframework.boot', 
            nexusUrl: '192.168.33.158:8081', 
            nexusVersion: 'nexus3', 
            protocol: 'http', 
            repository: 'nexus-test/', 
            version: '2.2.4'    
          }  
        }
 }
}
