pipeline {
    agent any
    tools {
        maven 'maven16'
    }
    stages {
        stage('build app') {
            steps {
                script {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }
        stage('test') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    sh 'docker build -t java-app .'
                }
            }
        }
        stage('push dockerhub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                        sh 'docker login --username $Username --password $Password'
                        sh 'docker tag java-app $Username/simple-java-app:latest'
                        sh 'docker push $Username/simple-java-app:latest'
                    }
                }
            }
        }
    }
}
