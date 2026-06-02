pipeline {
    agent any

    environment {
        IMAGE_NAME = 'java-app'
    }

    stages {

        stage('Test') {
            steps {
                script {
                    sh 'docker run --rm -v $(pwd):/app -w /app maven:3.6.3-jdk-11-slim mvn -f /app/pom.xml test'
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                        sh 'docker login --username $Username --password $Password'
                        sh 'docker tag $IMAGE_NAME $Username/$IMAGE_NAME'
                        sh 'docker push $Username/$IMAGE_NAME'
                    }
                }
            }
        }

    }
}
