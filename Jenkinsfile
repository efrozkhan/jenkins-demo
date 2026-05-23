pipeline {

    agent any

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/efrozkhan/jenkins-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t mysite:${BUILD_NUMBER} .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                docker stop mycontainer || true
                docker rm mycontainer || true
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d -p 80:80 --name mycontainer mysite:${BUILD_NUMBER}
                '''
            }
        }
    }
}
