pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'your-jenkins-dockerhub-credentials-id' // Update this with the ID you set in Jenkins credentials
        DOCKER_IMAGE = 'deepam-dits/pipeline-docker' // Replace with your DockerHub repo
    }

    stages {
        stage('Build') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE% .'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing index.html presence'
                bat 'if not exist index.html exit 1'
            }
        }
        stage('Login') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }
        stage('Package') {
            steps {
                bat 'docker tag %DOCKER_IMAGE% %DOCKER_IMAGE%:latest'
            }
        }
        stage('Deploy') {
            steps {
                bat 'docker run -d -p 80:80 %DOCKER_IMAGE%'
            }
        }
    }
}
