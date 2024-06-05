pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub') // Replace with your Jenkins credentials ID for Docker Hub
        DOCKERHUB_ACCOUNT = 'i211230' // Replace with your Docker Hub username
        REPO_NAME = 'SCD_Final_Exam' // Replace with your repository name
        DOCKER_REGISTRY_URL = 'docker.io' // Docker registry URL
        DOCKER_IMAGE_TAG = 'latest' // Tag for Docker images
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/AbdullahCheetah/SCD_Final_Exam.git'
            }
        }
        stage('Install Dependencies') {
            parallel {
                stage('Auth Service') {
                    agent {
                        docker {
                            image 'node:14-alpine'
                            args '-v /var/jenkins_home:/var/jenkins_home' // Adjust volume if needed
                        }
                    }
                    steps {
                        dir('Auth') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Classrooms Service') {
                    agent {
                        docker {
                            image 'node:14-alpine'
                            args '-v /var/jenkins_home:/var/jenkins_home' // Adjust volume if needed
                        }
                    }
                    steps {
                        dir('Classrooms') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Client Frontend') {
                    agent {
                        docker {
                            image 'node:14-alpine'
                            args '-v /var/jenkins_home:/var/jenkins_home' // Adjust volume if needed
                        }
                    }
                    steps {
                        dir('client') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Event Bus Service') {
                    agent {
                        docker {
                            image 'node:14-alpine'
                            args '-v /var/jenkins_home:/var/jenkins_home' // Adjust volume if needed
                        }
                    }
                    steps {
                        dir('event-bus') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Post Service') {
                    agent {
                        docker {
                            image 'node:14-alpine'
                            args '-v /var/jenkins_home:/var/jenkins_home' // Adjust volume if needed
                        }
                    }
                    steps {
                        dir('Post') {
                            sh 'npm install'
                        }
                    }
                }
            }
        }
        stage('Build Docker Images') {
            parallel {
                stage('Build Auth Image') {
                    steps {
                        script {
                            dockerImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_auth", 'Auth')
                        }
                    }
                }
                stage('Build Classrooms Image') {
                    steps {
                        script {
                            dockerImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_classrooms", 'Classrooms')
                        }
                    }
                }
                stage('Build Client Image') {
                    steps {
                        script {
                            dockerImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_client", 'client')
                        }
                    }
                }
                stage('Build Event Bus Image') {
                    steps {
                        script {
                            dockerImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_eventbus", 'event-bus')
                        }
                    }
                }
                stage('Build Post Image') {
                    steps {
                        script {
                            dockerImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_post", 'Post')
                        }
                    }
                }
            }
        }
        stage('Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        def authImage = docker.image("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_auth:${DOCKER_IMAGE_TAG}")
                        authImage.push()
                        
                        def classroomsImage = docker.image("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_classrooms:${DOCKER_IMAGE_TAG}")
                        classroomsImage.push()

                        def clientImage = docker.image("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_client:${DOCKER_IMAGE_TAG}")
                        clientImage.push()

                        def eventBusImage = docker.image("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_eventbus:${DOCKER_IMAGE_TAG}")
                        eventBusImage.push()

                        def postImage = docker.image("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_post:${DOCKER_IMAGE_TAG}")
                        postImage.push()
                    }
                }
            }
        }
    }
}
