pipeline {
    environment {
        registry = "sandeep4642/shop-example"
        registryCredential = 'dockerhub'
        dockerImage = ''
      }
    agent none
    stages {
        stage('mvn build'){
            agent { label "jenkins-docker-slave"}
            steps{
                sh 'mvn -DskipTests clean package'
            }
        }
        stage('Building Docker image') {
            agent { label "jenkins-docker-slave"}
            steps{
              script {
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
              }
            }
        
        }
        stage('Deploy push') {
            agent { label "jenkins-docker-slave"}
            steps{
              script {
                docker.withRegistry( '', registryCredential ) {
                  dockerImage.push()
                }
              }
            }
          }
        stage('Remove Unused docker image') {
            agent { label "jenkins-docker-slave"}
            steps{
              sh "docker rmi $registry:$BUILD_NUMBER"
            }
          }
          stage('Deploy to dev Server'){
              agent { label "ansible-master"}
            steps{
                sh "whoami"
                sh 'ansible-playbook /home/ubuntu/ansible/docker-deploy-dev.yml'
        }
     }
    }
}
