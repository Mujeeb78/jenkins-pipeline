pipeline {
    agent any
     options{
        timestamps ()
        timeout(time: 10, unit: 'SECONDS')
        skipDefaultCheckout true
        buildDiscarder(logRotator(daysToKeepStr: '2'))
    }
    stages {
        stage("Checking Docker Version") {
            steps {
                
                sh "docker --version"
                
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
                
        }}
        stage('deploy') {
            steps{
               {
                sh 'docker run -d --name container${BUILD_NUMBER} apacheimage44:44'
                sh 'docker container ls'
                }
            }}
             

        stage('Cleanup') {
            steps {
                sh 'docker rmi -f $(docker images -q)'
                sh 'docker images'
                sh 'docker image prune -f'
            }
                
            }
        
        }
    }

