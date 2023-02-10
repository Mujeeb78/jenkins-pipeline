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
        }
        }
