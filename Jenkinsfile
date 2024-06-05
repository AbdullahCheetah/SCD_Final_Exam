pipeline {
    agent any

    tools {
        nodejs 'jenkinsNode'
    }

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub') // Jenkins credentials for Docker Hub
        DOCKER_REGISTRY_URL = 'docker.io' // Docker registry URL
        DOCKER_IMAGE_TAG = 'latest' // Tag for Docker images
    }

    stages {
        stage('i211198_Checkout') {
            steps {
                git 'https://github.com/NUCESFAST/scd-final-lab-exam-MasoodGhauri'
            }
        }
        
        stage('i211198_Build and Push Auth Service') {
            steps {
                dir('Auth_i211198_backend') {
                    sh 'npm install'
                    sh "docker build -t masoodghauri/auth-service:${DOCKER_IMAGE_TAG} ."
                    withCredentials([usernamePassword(credentialsId: 'dockerHub_credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        echo "Logging in to Docker Hub with username: $DOCKER_HUB_USERNAME"
                        sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"
                        sh "docker push masoodghauri/auth-service:${DOCKER_IMAGE_TAG}"
                    }
                }
            }
        }
        
        stage('i211198_Build and Push Classroom Service') {
            steps {
                dir('Classrooms_i211198_backend') {
                    sh 'npm install'
                    sh "docker build -t masoodghauri/classroom-service:${DOCKER_IMAGE_TAG} ."
                    withCredentials([usernamePassword(credentialsId: 'dockerHub_credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"
                        sh "docker push masoodghauri/classroom-service:${DOCKER_IMAGE_TAG}"
                    }
                }
            }
        }
        
        stage('i211198_Build and Push Event-Bus Service') {
            steps {
                dir('event-bus_i211198_backend') {
                    sh 'npm install'
                    sh "docker build -t masoodghauri/event-bus-service:${DOCKER_IMAGE_TAG} ."
                    withCredentials([usernamePassword(credentialsId: 'dockerHub_credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"
                        sh "docker push masoodghauri/event-bus-service:${DOCKER_IMAGE_TAG}"
                    }
                }
            }
        }
        
        stage('i211198_Build and Push Post Service') {
            steps {
                dir('Post_i211198_backend') {
                    sh 'npm install'
                    sh "docker build -t masoodghauri/post-service:${DOCKER_IMAGE_TAG} ."
                    withCredentials([usernamePassword(credentialsId: 'dockerHub_credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"
                        sh "docker push masoodghauri/post-service:${DOCKER_IMAGE_TAG}"
                    }
                }
            }
        }
        
        stage('i211198_Build and Push Frontend Service') {
            steps {
                dir('i211198_frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                    sh "docker build -t masoodghauri/frontend-service:${DOCKER_IMAGE_TAG} ."
                    withCredentials([usernamePassword(credentialsId: 'dockerHub_credentials', usernameVariable: 'DOCKER_HUB_USERNAME', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        sh "echo $DOCKER_HUB_PASSWORD | docker login -u $DOCKER_HUB_USERNAME --password-stdin"
                        sh "docker push masoodghauri/frontend-service:${DOCKER_IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
