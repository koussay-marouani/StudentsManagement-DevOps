pipeline {
    agent any

    environment {
        REGISTRY = "https://index.docker.io/v1/"
        IMAGE_NAME = "koussaymarouani/studentsmanagement"
        DOCKER_CREDENTIALS = "dockerhub-creds"
        
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
        stage('Deploy to Kubernetes') {
    steps {
        sh '''
        kubectl apply -f k8s/namespace.yaml
        kubectl delete deployment spring-app -n devops --ignore-not-found=true
        kubectl apply -f k8s/mysql-deployment.yaml -n devops
        kubectl apply -f k8s/spring-deployment.yaml -n devops
        '''
    }
}



     }}
