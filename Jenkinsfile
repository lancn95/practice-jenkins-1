pipeline {
    agent any

    stages {
        stage('Example Username/Password') {
            environment {
                SERVICE_CREDS = credentials('credential-github')
            }
            steps {
                sh 'echo "Service user is $SERVICE_CREDS_USR"'
                sh 'echo "Service password is $SERVICE_CREDS_PSW"'
                sh 'curl -u $SERVICE_CREDS https://myservice.example.com'
            }
        }
        stage("Git Checkout"){  
            steps{     
	            git credentialsId: 'credential-github', url: 'https://github.com/lancn95/practice-jenkins-1'
	            echo 'Git Checkout Completed'   
            }
        }
        stage('Build Docker Image') {
            environment {
                BUILD_NUMBER = '1'
            }
            steps{
                sh 'sudo docker build -t lancn1/springboot-jenkins:$BUILD_NUMBER .'
                echo 'Build Image Completed'
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
