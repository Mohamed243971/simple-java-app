pipeline {
    agent any 

    stages {
        // 1. مرحلة الـ Build & Test (هنعملهم جوه حاوية مافن معزولة تماماً)
        stage('Build & Test') {
            steps {
                echo 'Building and Testing Java Application inside Docker Maven Container...'
                // الأمر ده بيقوم حاوية مافن، يعطيه الكود بتاعك، يشغل التيست والـ package، ويمسح الحاوية فوراً
                sh 'docker run --rm -v "$(pwd)":/app -w /app maven:3.6.3-jdk-11-slim mvn clean package'
            }
        }

        // 2. مرحلة بناء صورة الدكر النهائية للتطبيق
        stage('Build Docker Image') {
            steps {
                echo 'Building Final Docker Image...'
                script {
                    dockerImage = docker.build("java-app")
                }
            }
        }

        // 3. مرحلة الـ Push على Docker Hub
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
