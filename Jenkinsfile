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
                sh 'curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-17.04.0-ce.tgz \
                    && tar xzvf docker-17.04.0-ce.tgz \
                    && mv docker/docker /usr/local/bin \
                    && rm -r docker docker-17.04.0-ce.tgz'
                sh 'docker ps'
                sh 'docker build -t lancn1/springboot-jenkins:$BUILD_NUMBER .'
                echo 'Build Image Completed'
            }
        }
        stage('Login to Docker Hub') {
            environment {
                DOCKERHUB_CREDENTIALS = credentials('credential-dockerhub')
            }      	
            steps{          
                withDockerRegistry(credentialsId: 'credential-dockerhub', url: 'https://index.docker.io/v1/') {
                    // sh 'docker build -t khaliddinh/springboot .'
                    // sh 'docker push khaliddinh/springboot'
                }
                // sh 'docker login -u ${DOCKERHUB_CREDENTIALS_USR} -p ${DOCKERHUB_CREDENTIALS_PSW}'             	
                // sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                		
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
