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
                    credentialsId: 'dockerhub_creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )}) {

                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push efrozkhan6194/mysite:${BUILD_NUMBER}'
            }
        }
    
        stage('Deploy to k8s') {
            steps {
               sh '''
              kubectl apply -f k8s/deployment.yml
              kubectl apply -f k8s/service.yml
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
