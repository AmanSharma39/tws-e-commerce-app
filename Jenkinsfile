pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'amans333/easyshop-app'
        DOCKER_IMAGE_TAG = "${BUILD_NUMBER}"
        GITHUB_CREDENTIALS = credentials('GIthubCred')
        GIT_BRANCH = 'master'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: env.GIT_BRANCH,
                    url: 'https://github.com/AmanSharma39/tws-e-commerce-app.git',
                    credentialsId: env.GITHUB_CREDENTIALS
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Devops', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} .
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                    docker push ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}
                """
            }
        }
    }
}


