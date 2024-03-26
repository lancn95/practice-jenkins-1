pipeline {

    agent any
    tools {
            maven 'my-maven'
        }
    environment {
        MYSQL_ROOT_LOGIN = credentials('mysql-root-login')
    }
    stages {

        stage('Build with Gradle') {
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew --version'
                sh 'java -version'
                sh './gradlew clean build'
            }
        }

        stage('Packaging/Pushing image') {

            steps {
                withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t cnlan1/springboot .'
                    sh 'docker push cnlan1/springboot'
                }
            }
        }

        stage('Deploy MySQL to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker image pull mysql:8.0'
                sh 'docker network create dev || echo "this network exists"'
                sh 'docker container stop cnlan1-mysql || echo "this container does not exist" '
                sh 'echo y | docker container prune '
                sh 'docker volume rm cnlan1-mysql-data || echo "no volume"'

                sh "docker run --name cnlan1-mysql --rm --network dev -v cnlan1-mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_LOGIN_PSW} -e MYSQL_DATABASE=db_example  -d mysql:8.0 "
                sh 'sleep 20'
                sh "docker exec -i cnlan1-mysql mysql --user=root --password=${MYSQL_ROOT_LOGIN_PSW} < script"
            }
        }

        stage('Deploy Spring Boot to DEV') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker image pull cnlan1/springboot'
                sh 'docker container stop cnlan1-springboot || echo "this container does not exist" '
                sh 'docker network create dev || echo "this network exists"'
                sh 'echo y | docker container prune '

                sh 'docker container run -d --rm --name cnlan1-springboot -p 8081:8080 --network dev cnlan1/springboot'
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
