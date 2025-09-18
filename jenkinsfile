pipeline {
    agent any

    tools {
        maven 'Maven3'   // Jenkins me configure kiya hua Maven tool ka naam
        jdk 'Java17'     // Jenkins me configure kiya hua JDK ka naam
    }

    environment {
        DOCKER_IMAGE = "bankingapp:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Priyankamisal8830/Banking.git',
                    credentialsId: 'github-token'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f banking || true
                docker run -d --name banking -p 8082:8082 $DOCKER_IMAGE
                '''
            }
        }
    }
}

