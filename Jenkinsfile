jenkins file:
pipeline {
  
        agent any
    tools {
        maven 'maven16' // لازم يكون نفس الاسم اللي كتبته في الإعدادات بالظبط
    }
    
    stages {
        // 1. Build Java App
        stage('build app') {
            steps {
                script {
                    // بنعمل Build للكود ونطلع الـ .jar file من غير ما نشغل الـ tests مؤقتاً
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        // 2. Test Java App
        stage('test') {
            steps {
                script {
                    // بنشغل الـ tests الخاصة بالتطبيق
                    sh 'mvn test'
                }
            }
        }

        // 3. Build Docker Image
        stage('build image') {
            steps {
                script {
                    sh 'docker build -t java-app .'
                }
            }
        }

        // 4. Push to DockerHub
        stage('push dockerhub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                        sh 'docker login --username $Username --password $Password'
                        
                        // يفضل نستخدم اسم الـ Repo الأصلي بتاعك في التاج
                        sh 'docker tag java-app $Username/simple-java-app:latest'
                        sh 'docker push $Username/simple-java-app:latest'
                    }
                }
            }
        }

    }
}
