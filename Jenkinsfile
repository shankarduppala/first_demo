pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shankarrao/myapp"
        TAG = "v1"
    }

    stages {
        
        stage('checkout SCM') {
            steps{
                git branch: 'main',
                    url: 'https://github.com/shankarduppala/first_demo.git',
                    credentialsId: 'Git-Creds'
            }
        }

        stage('Build image') {
            steps{
                bat '''
                docker build -t %DOCKER_IMAGE%:%TAG% .
                '''
            }
        }

        stage('Docker login & push ') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push %DOCKER_IMAGE%:%TAG%
                    '''
                }
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}