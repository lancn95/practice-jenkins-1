pipeline {

    agent any


    stages {
        environment {
          NAME: Kevin
          LASTNAME: CAT NGOC LAN
        }
        stage('Build Info'){
            steps {
                sh 'echo $NAME $LASTNAME'

            }
        }
        stage('Build with Gradle') {
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew --version'
                sh 'java -version'
                sh './gradlew clean build'
            }
        }


 
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}
