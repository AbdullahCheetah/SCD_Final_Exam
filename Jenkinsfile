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
                    steps {
                        dir('Auth') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Classrooms Service') {
                    steps {
                        dir('Classrooms') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Client Frontend') {
                    steps {
                        dir('client') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Event Bus Service') {
                    steps {
                        dir('event-bus') {
                            sh 'npm install'
                        }
                    }
                }
                stage('Post Service') {
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
                        def authImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_auth", 'Auth')
                        authImage.push()
                        
                        def classroomsImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_classrooms", 'Classrooms')
                        classroomsImage.push()

                        def clientImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_client", 'client')
                        clientImage.push()

                        def eventBusImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_eventbus", 'event-bus')
                        eventBusImage.push()

                        def postImage = docker.build("${DOCKERHUB_ACCOUNT}/${REPO_NAME}_post", 'Post')
                        postImage.push()
                    }
                }
            }
        }
    }
}
