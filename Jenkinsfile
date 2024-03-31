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
        stage('SonarQube Quality Check') {
            withSonarQubeEnv(installationName: 'sonar-community', credentialsId: 'jenkins-sonar-token') {
            sh 'mvn sonar:sonar'
            } // submitted SonarQube taskId is automatically attached to the pipeline context
        }
        stage('Show image tag'){
            steps {
                script {
                    def now = new Date()
                    println now.format("yyyyMMdd.HHmmss", TimeZone.getTimeZone('UTC'))
                }
                sh 'curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-17.04.0-ce.tgz \
                    && tar xzvf docker-17.04.0-ce.tgz \
                    && mv docker/docker /usr/local/bin \
                    && rm -r docker docker-17.04.0-ce.tgz'
                sh 'docker --version'
                echo 'Install Docker Completed'
            } 
            // steps {
                // sh 'curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-17.04.0-ce.tgz \
                //     && tar xzvf docker-17.04.0-ce.tgz \
                //     && mv docker/docker /usr/local/bin \
                //     && rm -r docker docker-17.04.0-ce.tgz'
                // sh 'docker --version'
                // echo 'Install Docker Completed'
            // }
            // steps {
            //     echo '$IMAGE_TAG'
            // }
        }
        stage('Build Docker Image') {
            // environment {
            //     BUILD_NUMBER = 'v1'
            // }
            steps{
                // sh 'docker build -t lancn1/springboot-jenkins:$BUILD_NUMBER .'
                sh 'docker build -t lancn1/springboot-jenkins .'
                echo 'Build Image Completed'
            }
        }
        // stage('Login to Docker Hub') {
     	//     environment {
        //         BUILD_NUMBER = 'v1'
        //     }
        //     steps{          
        //         // This step should not normally be used in your script. Consult the inline help for details .
        //         withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
        //             sh 'docker build -t lancn1/springboot-jenkins .'
        //             sh 'docker image ls'
        //             // sh 'docker tag $BUILD_NUMBER lancn1/springboot-jenkins'
        //             // sh 'docker push lancn1/springboot-jenkins'
        //             sh 'docker tag lancn1/springboot-jenkins lancn1/springboot-jenkins'
        //             sh 'docker image push lancn1/springboot-jenkins'
        //         }
                               		
        //         echo 'Login Completed'      
        //     }           
        // }  
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
