pipeline {
    agent any
    environment {
            BUILD_NUMBER = '1'
        }
    stages {
        stage("Git Checkout"){  
            steps{     
	            git credentialsId: 'lancn-github', url: 'https://github.com/lancn95/practice-jenkins-1'
	            echo 'Git Checkout Completed'   
            }
        stage('Build Docker Image') {
            steps{
              sh 'sudo docker build -t lancn1/springboot-jenkins:$BUILD_NUMBER .'
              echo 'Build Image Completed'
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
