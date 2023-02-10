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
            when{
            branch "main"
            }
            steps{
                sh 'docker build -t apacheimage${BUILD_NUMBER}:${BUILD_NUMBER} .'
                sh 'docker images'
//                 sh 'docker image inspect apacheimage:2'
//                  sh 'docker kill $(docker ps -q)'
                sh 'docker rmi -f $(docker images -q)'
//                 sh 'docker rm $(docker ps -a -q)'
//                 sh 'docker container ls'
                sh 'docker images'
            }
        }
        stage('deploy') {
            steps{
          {
                sh 'docker run -d --name container${BUILD_NUMBER} apacheimage:2'
                sh 'docker container ls'
                }
            }
        }      
    }

        stage('Cleanup') {
            steps {
                sh 'docker image prune -f'
            }
        }
    }

