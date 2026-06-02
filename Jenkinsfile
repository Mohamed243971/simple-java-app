pipeline {
    agent any
     tools {
        maven 'maven:3.6.3-jdk-11-slim'
    }
    stages {
        stage('build') {
            steps {
                sh 'mvn clean package -DskipTests'
                sh 'docker build -t mohammedmostafanada/java-app .'
            }
        }

        stage('Test') {
            steps {
                sh 'test -f target/*.jar'
            }
        }
        stage('Push image') {
            steps {
                 withCredentials([usernamePassword(
                    credentialsId: 'docker-hub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push mohammedmostafanada/java-app
                    '''
                }
            }
        }
    }
}
