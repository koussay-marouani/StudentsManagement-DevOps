pipeline {
    agent any

    environment {
        REGISTRY = "https://index.docker.io/v1/"
        IMAGE_NAME = "koussaymarouani/studentsmanagement"
        DOCKER_CREDENTIALS = "dockerhub-creds"
        SONAR_HOST_URL = "http://localhost:9000"
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/koussay-marouani/StudentsManagement-DevOps.git'
            }
        }

        stage('Clean') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                script {
                    docker.withRegistry(REGISTRY, DOCKER_CREDENTIALS) {
                        dockerImage.push("${BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('MVN SONARQUBE') {
    steps {
        withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
            sh """
                mvn sonar:sonar \
                -Dsonar.projectKey=student-management \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.login=${SONAR_TOKEN}
            """
        }
    }
}
}}
