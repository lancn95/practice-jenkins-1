pipeline {

    agent any


    stages {

        stage('Build with Gradle') {
            steps {
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