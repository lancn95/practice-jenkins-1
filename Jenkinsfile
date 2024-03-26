pipeline {
    agent any

    environment {
        DOCKER_REGISTRY_CREDENTIALS = credentials('docker-hub-credentials') // Add Docker Hub credentials
        DOCKER_IMAGE_NAME = "lancn1/springboot" // Define your Docker image name
        DOCKERFILE_PATH = "./Dockerfile" // Path to your Dockerfile
        MAVEN_HOME = tool 'Maven' // Use Maven tool installed in Jenkins
        DOCKER_HOME = tool 'Docker' // Use Docker tool installed in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                echo "${DOCKER_IMAGE_NAME}"
                echo "${MAVEN_HOME}"
                echo "${DOCKER_HOME}"
                echo "${DOCKERFILE_PATH}"
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the Spring Boot application
                    sh "${MAVEN_HOME}/bin/mvn clean package"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "${DOCKER_HOME}/bin/docker build -t ${DOCKER_IMAGE_NAME} -f ${DOCKERFILE_PATH} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub
                    withCredentials([string(credentialsId: DOCKER_REGISTRY_CREDENTIALS, variable: 'DOCKER_PASSWORD')]) {
                        sh "${DOCKER_HOME}/bin/docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    }
                    // Push Docker image to Docker Hub
                    sh "${DOCKER_HOME}/bin/docker push ${DOCKER_IMAGE_NAME}"
                }
            }
        }
    }

    post {
        success {
            echo 'Build and push to Docker Hub successful'
        }
        failure {
            echo 'Build or push to Docker Hub failed'
        }
    }
}
