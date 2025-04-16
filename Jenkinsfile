pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'amans333/easyshop-app'
        DOCKER_IMAGE_TAG = "${BUILD_NUMBER}"
        GITHUB_CREDENTIALS = credentials('GIthubCred')
        DOCKER_HUB_CREDENTIALS = credentials('Devops') // 👈 using 'devops' as DockerHub credentials
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
                script {
                    sh """
                        echo "${DOCKER_HUB_CREDENTIALS_PSW}" | docker login -u "${DOCKER_HUB_CREDENTIALS_USR}" --password-stdin
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} .
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh """
                        docker push ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}
                    """
                }
            }
        }
    }
}

