pipeline {
    agent any
    options {
        timestamps ()
        timeout(time: 10, unit: 'MINUTES')
        skipDefaultCheckout true
        buildDiscarder(logRotator(daysToKeepStr: '2'))
    }
    stages {
        stage('Checking Docker Version') {
            steps {
                retry(3){
                    sh 'dock --version'
                }
            }
        }
        stage('Checking GIT Version') {
            steps {
                sh 'git --version'
            }
        }
        stage('Build Docker Image'){
            steps{
                sh 'docker build -t apacheimage${BUILD_NUMBER}:${BUILD_NUMBER} .'
                sh 'docker images'
                sh 'docker image inspect apacheimage${BUILD_NUMBER}:${BUILD_NUMBER}'
            }
        }
        stage("Manual Approval") {
            steps{
                input 'Do you want to deploy this Docker image?'
            }
        }

        stage('Deploy Docker Image') {
            steps{
                sh 'docker run -d --name container${BUILD_NUMBER} apacheimage${BUILD_NUMBER}:${BUILD_NUMBER}'
                sh 'docker ps'
            }
        }
        stage('Cleanup') {
            steps {
                sh 'docker kill $(docker ps -q)' // to kill the conatainers
                sh 'docker rm $(docker ps -a -q)' // to remove the conatiners
                sh 'docker ps' // to list down the conatiners
                sh 'docker rmi -f $(docker images -q)' // to remove the images
                sh 'docker images' // to display the images
            //              sh 'docker image prune -f' // Removes unused images
            //              sh 'docker images'
            }
        }
    }
    post {
        success {
            echo "success"
        }
        failure {
            echo "failure"
        }
        unstable {
            echo "unstable"
        }
        aborted {
            echo "aborted"
        }
    }
}
