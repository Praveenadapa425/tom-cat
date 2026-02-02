pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Praveenadapa425/tom-cat.git'
            }
        }

        stage('Start Tomcat') {
            steps {
                sh '''
                docker stop tomcat-prod || true
                docker rm tomcat-prod || true
                docker run -d --name tomcat-prod -p 9090:8080 tomcat:9.0
                '''
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                docker exec tomcat-prod rm -rf /usr/local/tomcat/webapps/web-app
                docker exec tomcat-prod mkdir -p /usr/local/tomcat/webapps/web-app

                tar -cf - *.html css js img lib | docker exec -i tomcat-prod tar -xf - -C /usr/local/tomcat/webapps/web-app
                '''
            }
        }
    }
}
