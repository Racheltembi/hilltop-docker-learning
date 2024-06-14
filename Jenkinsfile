pipeline {

    agent any

    // Rename the username 'michaelgwei86' with your Docker Hub repo username
    environment {
        DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_CREDENTIALS')
        IMAGE_REPO_NAME = "rachelndah/class2024a-img"
        CONTAINER_NAME = "hilltopconsultancy_class2024a-cont"
    }

    stages {
        stage('Git checkout') {
            steps {
                echo 'Cloning project codebase...'
                git branch: 'main', url: 'https://github.com/Racheltembi/hilltop-docker-learning.git'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    def imageTag = "${IMAGE_REPO_NAME}:${BUILD_NUMBER}"
                    sh "docker build -t ${imageTag} ."
                    sh "docker images"
                }
            }
        }

        stage('Login to Dockerhub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DOCKERHUB_CREDENTIALS', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    def containerName = "${CONTAINER_NAME}-${BUILD_NUMBER}"
                    def imageTag = "${IMAGE_REPO_NAME}:${BUILD_NUMBER}"
                    sh "docker run --name ${containerName} -p 8089:8080 -d ${imageTag}"
                    sh "docker ps"
                }
            }
        }

        stage('Push to Dockerhub') {
            steps {
                script {
                    def imageTag = "${IMAGE_REPO_NAME}:${BUILD_NUMBER}"
                    sh "docker push ${imageTag}"
                }
            }
        }
    }
}
