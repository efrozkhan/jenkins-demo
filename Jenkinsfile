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
                sh 'docker build -t efrozkhan6194/mysite:${BUILD_NUMBER} .'
            }
        }

        stage('DockerHub Login') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push efrozkhan6194/mysite:${BUILD_NUMBER}

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop mycontainer || true
                sleep 3

                docker rm -f mycontainer || true
                sleep 5
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d -p 80:80 \
                --name mycontainer \
                YOUR_DOCKERHUB_USERNAME/mysite:${BUILD_NUMBER}
                '''
            }
        }

    }
}
