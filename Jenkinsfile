pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'amans333/easyshop-app'
        DOCKER_MIGRATION_IMAGE_NAME = 'amans333/easyshop-migration'
        DOCKER_IMAGE_TAG = "${BUILD_NUMBER}"
        GITHUB_CREDENTIALS = credentials('GIthubCred')
        GIT_BRANCH = "master"
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: env.GIT_BRANCH,
                    url: 'https://github.com/LondheShubham153/tws-e-commerce-app.git',
                    credentialsId: env.GITHUB_CREDENTIALS
            }
        }

        stage('Build Docker Images') {
            parallel {
                stage('Build Main App Image') {
                    steps {
                        script {
                            sh """
                                docker build -t amans333/easyshop-app .
                            """
                        }
                    }
                }

                stage('Build Migration Image') {
                    steps {
                        script {
                            sh """
                                docker build -t ${DOCKER_MIGRATION_IMAGE_NAME}:${DOCKER_IMAGE_TAG} -f scripts/Dockerfile.migration .
                            """
                        }
                    }
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    sh 'echo "Running unit tests..."'
                    // Add real test commands here
                }
            }
        }

        stage('Push Docker Images') {
            parallel {
                stage('Push Main App Image') {
                    steps {
                        withCredentials([usernamePassword(credentialsId: 'Devops', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                            sh """
                                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                                docker push ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}
                            """
                        }
                    }
                }

                // Uncomment when you want to push migration image too
                // stage('Push Migration Image') {
                //     steps {
                //         withCredentials([usernamePassword(credentialsId: 'Devops', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                //             sh """
                //                 echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                //                 docker push ${DOCKER_MIGRATION_IMAGE_NAME}:${DOCKER_IMAGE_TAG}
                //             """
                //         }
                //     }
                // }
            }
        }
    }
}

