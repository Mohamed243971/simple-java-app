pipeline {
    agent any 

    stages {
        stage('Build & Test') {
            steps {
                echo 'Building and Testing Java Application inside Docker Maven Container...'
                sh "docker run --rm -v ${WORKSPACE}:/app -w /app maven:3.6.3-jdk-8-slim mvn clean package"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Final Docker Image...'
                script {
                    dockerImage = docker.build("java-app")
                }
            }
        }

        stage('push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                        sh 'docker login --username \$Username --password \$Password'
                        sh 'docker tag java-app \$Username/java-app:latest'
                        sh 'docker push \$Username/java-app:latest'
                    }
                }
            }
        }
    }
}
