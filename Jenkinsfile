pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/Biswa388/Docker-web-app.git'
            }
        }

        stage('Build WAR using Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build App Docker Image') {
            steps {
                sh '''
                cd Docker-app
                cp -r ../target .
                docker build -t appimage .
                '''
            }
        }

        stage('Build DB Docker Image') {
            steps {
                sh '''
                cd Docker-db
                docker build -t dbimage .
                '''
            }
        }

        stage('Run Containers') {
            steps {
                sh '''
                docker rm -f devopsdb devopsweb || true

                docker run -itd \
                --name devopsdb \
                -p 3306:3306 \
                dbimage

                docker run -itd \
                --name devopsweb \
                -p 8081:8080 \
                --link devopsdb:mysql \
                appimage
                '''
            }
        }
    }
}

