pipeline {
    agent any

    environment {
        SERVICE = "EurekaServer"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out code..."
                git branch: 'master',
                    url: 'https://github.com/PawanKumar753/VehicleInsurence_MS.git',
                    credentialsId: 'springboot-jenkins4'
            }
        }

        stage('Build JAR') {
            steps {
                echo "Building JAR for ${env.SERVICE}..."
                bat "mvn -f %WORKSPACE%\\${env.SERVICE}\\pom.xml clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image for ${env.SERVICE}..."
                bat "docker build -t ${env.SERVICE.toLowerCase()}:latest %WORKSPACE%\\${env.SERVICE}"
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Stopping existing container if exists..."
                bat "docker rm -f ${env.SERVICE.toLowerCase()} || echo Container not found"

                echo "Running container..."
                bat "docker run -d -p 8761:8761 --name ${env.SERVICE.toLowerCase()} ${env.SERVICE.toLowerCase()}:latest"
            }
        }
    }

    post {
        success {
            echo "${env.SERVICE} deployed successfully!"
        }
        failure {
            echo "Deployment failed for ${env.SERVICE}!"
        }
    }
}
