pipeline {
    agent any
    stages {
        stage("Checking Docker Version") {
            steps {
                retry(3){
                sh "docker --version"
                }
            }
        }
        stage("Checking GIT Version") {
            steps {
                sh "git --version"
            }
        }
        stage('Build Docker Image'){
            steps{
                sh 'docker build -t apacheimage${BUILD_NUMBER}:${BUILD_NUMBER} .'
                sh 'docker images'
                
                 }
        }
        stage('Deploy Docker Image') {
            steps{
               
                sh 'docker run -d --name container${BUILD_NUMBER} apacheimage13:13'
                sh 'docker container ls'
            }
        }
        stage('Cleanup') {
            steps {
                // sh 'docker rmi -f $(docker images -q)'
                sh 'docker images'
                sh 'docker image prune -f'
                sh 'docker images'
            }
        }
    }
}
