pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDS = 'docker_hub_credential'
        DOCKER_IMAGE = 'ahmedmaher4/orange_project_weather-app:latest'
    }

    stages {
        
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/ahmedMaher999/DevOps-Project_Orange-internship.git', 
                    credentialsId: 'access private repo on github'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${DOCKER_IMAGE} .
                '''
            }
        }

        stage('Login To Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_HUB_CREDS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                    echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                    '''
                }
            }
        }

        stage('Push The Image To Dockerhub') {
            steps {
                sh 'docker push ${DOCKER_IMAGE}'
            }
        }
    }
}