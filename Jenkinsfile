pipeline {
    agent any
    environment {
        IMAGE = "koussaymarouani/studentsmanagement:latest"
        NAMESPACE = "devops"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Maven') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl set image deployment/spring-app spring=$IMAGE -n $NAMESPACE"
            }
        }
    }
    post {
        success {
            echo 'Deployment to Kubernetes successful!'
        }
    }
}

