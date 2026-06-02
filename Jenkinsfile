pipeline {
    agent any

    environment {
        IMAGE_NAME = 'java-app'
    }

    stages {

        stage('Build App') {
            steps {
                script {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh 'mvn test'
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
