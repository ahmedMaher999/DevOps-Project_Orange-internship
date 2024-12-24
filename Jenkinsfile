pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDS = 'docker_hub_credential'
        DOCKER_IMAGE = 'ahmedmaher4/orange_project_weather-app:latest'
        PRIVATE_KEY_1 = 'keys/prinvate_key_1'
        PRIVATE_KEY_2 = 'keys/prinvate_key_2'
    }

    stages {
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

        stage ('Run Playbook') {
            steps {
                script {
                    sh '''
                    chmod 600 $PRIVATE_KEY_1

                    chmod 600 $PRIVATE_KEY_2

                    ansible-playbook -i inventory.ini playbook.yml \
                    --private-key $PRIVATE_KEY_1 --limit 192.168.45.30

                    ansible-playbook -i inventory.ini playbook.yml \
                    --private-key $PRIVATE_KEY_1 --limit 192.168.45.31
                    '''
                }
            }
        }
    }
}