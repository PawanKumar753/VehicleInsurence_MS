pipeline {
    agent {
        docker {
            image 'maven:3.9.3-eclipse-temurin-17' // Maven + JDK 17
            args '-v /var/run/docker.sock:/var/run/docker.sock' // to run Docker inside Docker
        }
    }

    environment {
        SERVICE = 'EurekaServer'
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
                sh "mvn -f ${env.SERVICE}/pom.xml clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = env.SERVICE.toLowerCase() + ":latest"
                    echo "Building Docker image: ${imageName}..."
                    sh """
                    docker build -t ${imageName} -f ${env.SERVICE}/Dockerfile ${env.SERVICE}
                    """
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    def containerName = env.SERVICE.toLowerCase()
                    echo "Stopping existing container if exists..."
                    sh "docker rm -f ${containerName} || true"

                    echo "Running container: ${containerName}..."
                    sh "docker run -d -p 8761:8761 --name ${containerName} ${env.SERVICE.toLowerCase()}:latest"
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
