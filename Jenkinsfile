pipeline {
    agent any 

    stages {
        stage('build') {
            steps {
                echo 'Building Java Application using Maven...'
                sh 'mvn clean package -DskipTests' 
            }
        }

        stage('Test') {
            steps {
                echo 'Running Unit Tests...'
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
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
