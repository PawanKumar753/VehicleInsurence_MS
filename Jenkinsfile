pipeline {
    agent any

    environment {
        SERVICES = ['EurekaServer']
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', 
                    url: 'https://github.com/PawanKumar753/VehicleInsurence_MS.git', 
                    credentialsId: 'springboot-jenkins4'
            }
        }

        stage('Build JARs') {
            steps {
                script {
                    SERVICES.each { service ->
                        echo "Building JAR for ${service}..."
                        bat "mvn -f ${service}/pom.xml clean package -DskipTests"
                    }
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    SERVICES.each { service ->
                        def imageName = service.toLowerCase() + ":latest"
                        echo "Building Docker image for ${service} as ${imageName}..."
                        bat "docker build -t ${imageName} -f \"%WORKSPACE%\\${service}\\Dockerfile\" \"%WORKSPACE%\\${service}\""
                    }
                }
            }
        }

        stage('Run Docker Containers') {
            steps {
                script {
                    SERVICES.each { service ->
                        def imageName = service.toLowerCase() + ":latest"
                        echo "Running Docker container for ${service}..."
                        // Stop existing container if it exists
                        bat "docker rm -f ${service.toLowerCase()} || echo Container not found"
                        // Run new container
                        bat "docker run -d -p 8761:8761 --name ${service.toLowerCase()} ${imageName}"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'All services deployed successfully!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
