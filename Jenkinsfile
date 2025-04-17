pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'amans333/easyshop-app'
        DOCKER_IMAGE_TAG = 'latest'
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

        stage('Build Main App Image') {
            steps {
                script {
                    sh """
                    docker build -t ${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG} -f Dockerfile .
                    """
                }
            }
        }

        stage('Push Main App Image') {
            steps {
                script {
                    sh """
                    echo "${DOCKER_HUB_PASSWORD}" | docker login -u "${DOCKER_HUB_USERNAME}" --password-stdin
                    docker push ${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG}
                    """
                }
            }
        }
    }
}


