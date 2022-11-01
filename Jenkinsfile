pipeline {
    environment {
        registry = "sandeep4642/shop-example"
        registryCredential = 'dockerhub'
        dockerImage = ''
      }
    agent { label "slave-1"}
    stages {
        stage('mvn build'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Building Docker image') {
            steps{
              script {
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
              }
            }
        
        }
        stage('Deploy push') {
            steps{
              script {
                docker.withRegistry( '', registryCredential ) {
                  dockerImage.push()
                }
              }
            }
          }
        stage('Remove Unused docker image') {
            steps{
              sh "docker rmi $registry:$BUILD_NUMBER"
            }
          }
     }
    }
