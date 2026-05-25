pipeline {

    agent any

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/efrozkhan/jenkins-demo.git'
            }
        }

        stage('Check Workspace') {
            steps {
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Docker Info') {
            steps {
                sh 'docker --version'
                sh 'docker images'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build \
                -t efrozkhan6194/mysite:${BUILD_NUMBER} \
                -t efrozkhan6194/mysite:latest .
                '''
            }
        }

        stage('DockerHub Login') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub_creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Images') {
            steps {

                sh '''
                docker push efrozkhan6194/mysite:${BUILD_NUMBER}
                docker push efrozkhan6194/mysite:latest
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {

                sh '''
                kubectl apply --validate=false -f k8s/deployment.yml
                kubectl apply --validate=false -f k8s/service.yml
                '''
            }
        }

        stage('Update Kubernetes Image') {
            steps {

                sh '''
                kubectl set image deployment/mysite-deployment \
                mysite=efrozkhan6194/mysite:${BUILD_NUMBER}
                '''
            }
        }

    }
}
