pipeline {

    agent any

    environment {
        MYSQL_ROOT_LOGIN = credentials('mysql-root-login')
    }
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
