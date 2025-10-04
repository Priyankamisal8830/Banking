pipeline {
    agent any

    tools {
        maven 'Maven3'    // Jenkins में configured Maven tool name
        jdk 'Java17'      // Jenkins में configured JDK tool name
    }

    environment {
        DOCKER_IMAGE = "bankingapp:latest"
        MAVEN_REPO = "/var/lib/jenkins/.m2/repository"
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    branch: 'master',
                    url: 'https://github.com/Priyankamisal8830/Banking.git',
                    credentialsId: 'github-token',
                    changelog: false,
                    poll: false
                )
            }
        }

        stage('Build with Maven') {
            steps {
                echo "🧱 Building project with Maven..."
                sh """
                mvn clean package -DskipTests -Dmaven.repo.local=$MAVEN_REPO
                """
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                sh """
                docker build -t $DOCKER_IMAGE . --no-cache=false
                """
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "🚀 Deploying Docker container..."
                sh """
                docker rm -f banking || true
                docker run -d --name banking -p 8082:8082 $DOCKER_IMAGE
                """
            }
        }
    }

    post {
        success {
            echo "✅ Build and deployment completed successfully!"
        }
        failure {
            echo "❌ Build or deployment failed. Check logs above."
        }
    }
}

