pipeline {
    agent any

    environment {
        // You can define single service here; later, you can change to a list inside script
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
                script {
                    def imageName = env.SERVICE.toLowerCase() + ":latest"
                    echo "Building Docker image: ${imageName}..."
                    bat "docker build -t ${imageName} -f \"%WORKSPACE%\\${env.SERVICE}\\Dockerfile\" \"%WORKSPACE%\\${env.SERVICE}\""
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    def containerName = env.SERVICE.toLowerCase()
                    echo "Stopping existing container if exists..."
                    bat "docker rm -f ${containerName} || echo Container not found"

                    echo "Running container: ${containerName}..."
                    bat "docker run -d -p 8761:8761 --name ${containerName} ${env.SERVICE.toLowerCase()}:latest"
                }
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
