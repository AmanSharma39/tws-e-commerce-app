
pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'amans333/easyshop-app'
        DOCKER_IMAGE_TAG = "${BUILD_NUMBER}"
        GITHUB_CREDENTIALS = credentials('GIthubCred')
        GIT_BRANCH = "master"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: env.GIT_BRANCH,
                    url: 'https://github.com/AmanSharma39/tws-e-commerce-app.git',
                    credentialsId: env.GITHUB_CREDENTIALS
            }
        }

        stage('Build Docker Images') {
            parallel {
                stage('Build Main App Image') {
                    steps {
                        script {
                            docker_build(
                                imageName: env.DOCKER_IMAGE_NAME,
                                imageTag: env.DOCKER_IMAGE_TAG,
                                dockerfile: 'Dockerfile',
                                context: '.'
                            )
                        }
                    }
                }
            }
        }

        stage('Push Docker Images') {
            parallel {
                stage('Push Main App Image') {
                    steps {
                        script {
                            docker_push(
                                imageName: env.DOCKER_IMAGE_NAME,
                                imageTag: env.DOCKER_IMAGE_TAG,
                                credentials: 'Docker'
                            )
                        }
                    }
                }
            }
        }
    }
}
