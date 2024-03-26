pipeline {
    agent any
    tools {
        maven 'my-maven-3.9.6' 
    }
    stages {
        // stage('Example Username/Password') {
        //     environment {
        //         SERVICE_CREDS = credentials('credential-github')
        //     }
        //     steps {
        //         sh 'echo "Service user is $SERVICE_CREDS_USR"'
        //         sh 'echo "Service password is $SERVICE_CREDS_PSW"'
        //     }
        // }
        // stage("Git Checkout"){  
        //     steps{     
	    //         git credentialsId: 'credential-github', url: 'https://github.com/lancn95/practice-jenkins-1'
	    //         echo 'Git Checkout Completed'   
        //     }
        // }
        stage('Build with Maven') {
            steps {
                sh 'mvn --version'
                sh 'java -version'
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }
        stage('Build Docker Image') {
            environment {
                BUILD_NUMBER = '1'
            }
            steps{
                sh 'docker build -t lancn1/springboot-jenkins:$BUILD_NUMBER .'
                echo 'Build Image Completed'
            }
        }
        stage('Login to Docker Hub') {
            environment {
                DOCKERHUB_CREDENTIALS = credentials('credential-dockerhub')
            }      	
            steps{                       	
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                		
                echo 'Login Completed'      
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
