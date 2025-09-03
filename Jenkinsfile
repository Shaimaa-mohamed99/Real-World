pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKER_IMAGE = "shaimaa9/real-world"
    }

    tools {
        maven 'Maven'
    }

    stages {
        stage('Pull code') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build & Push Docker image') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE:${env.BUILD_NUMBER} ."
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push $DOCKER_IMAGE:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}
