pipeline {
    agent any

    stage("Git Checkout"){  
    steps{     
	git credentialsId: 'lancn-github', url: 'https://github.com/lancn95/practice-jenkins-1'
	echo 'Git Checkout Completed'   
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
