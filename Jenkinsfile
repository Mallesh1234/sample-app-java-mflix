pipeline {
    agent any

    environment {
        IMAGE_NAME = "springboot-demo"
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Mallesh1234/sample-app-java-mflix.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Deploy to Kubernetes (Helm)') {
            steps {
                sh '''
                helm upgrade --install springboot-app helm/springboot-helm \
                  --set image.repository=${IMAGE_NAME} \
                  --set image.tag=${IMAGE_TAG}
                '''
            }
        }
    }
}
