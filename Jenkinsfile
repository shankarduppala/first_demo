pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shankarrao/myapp"
        TAG = "${BUILD_NUMBER}"
    }

    stages {
        
        stage('checkout SCM') {
            steps{
                git 'https://github.com/shankarduppala/first_demo.git'
            }
        }

        stage('Build image') {
            steps{
                sh 'docker build -t $DOCKER-IMAGE:$TAG .'
            }
        }

        stage('Docker login & push ') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push $DOCKER_IMAGE:$TAG
                    '''
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}