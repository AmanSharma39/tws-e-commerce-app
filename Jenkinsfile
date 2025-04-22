@Library('Shared') _

pipeline {
    agent any
    
    environment {
        // Update the main app image name to match the deployment file
        DOCKER_IMAGE_NAME = 'amans333/easyshop-app'
        DOCKER_IMAGE_TAG = "${BUILD_NUMBER}"
        GITHUB_CREDENTIALS = credentials('github-credentials')
        GIT_BRANCH = "master"
    }
    
    stages {
        stage('Cleanup Workspace') {
            steps {
                script {
                    cleanupWorkspace()
                }
            }
        }
        
        stage('Clone Repository') {
            steps {
                script {
                    checkoutRepo("https://github.com/AmanSharma39/tws-e-commerce-app.git", "master")
                }
            }
        }
        
        stage('Build Docker Images') {
            steps {
                script {
                    buildDockerImage(
                        imageName: env.DOCKER_IMAGE_NAME,
                        imageTag: env.DOCKER_IMAGE_TAG,
                        dockerfile: 'Dockerfile',
                        context: '.'
                    )
                }
            }
        }
        
        stage('Push Docker Images') {
            steps {
                script {
                    pushDockerImage(
                        imageName: env.DOCKER_IMAGE_NAME,
                        imageTag: env.DOCKER_IMAGE_TAG,
                        credentials: 'docker-hub-credentials'
                    )
                }
            }
        }
        
        // Optional stage for security scan (Trivy)
        // stage('Security Scan with Trivy') {
        //     steps {
        //         script {
        //             trivyScan()
        //         }
        //     }
        // }
    }
}
