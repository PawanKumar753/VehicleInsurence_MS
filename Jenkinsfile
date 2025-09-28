 
pipeline {
    agent any

    tools {
        maven 'Maven-3.9'  // Jenkins Maven tool name
        jdk 'Java17'           // Jenkins JDK tool name
    }

    environment {
        SERVICE_NAME = "eurekaserver"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/PawanKumar753/VehicleInsurence_MS.git'
            }
        }

        stage('Build JAR') {
            steps {
                bat 'mvn -f EurekaServer/pom.xml clean package -DskipTests'
            }
        }

       stage('Build Docker Image') {
            steps {
                bat 'docker build -t eurekaserver:latest -f "C:/ProgramData/Jenkins/.jenkins/workspace/SpringBoot-Build2/EurekaServer/Dockerfile" "C:/ProgramData/Jenkins/.jenkins/workspace/SpringBoot-Build2/EurekaServer"'
            }
        }



        stage('Run Docker Container') {
            steps {
                // Run the container
                bat """
                docker run -d -p 8761:8761 \
                --name ${SERVICE_NAME} \
                ${SERVICE_NAME}:latest
                """
            }
        }
    }

    post {
        always {
            echo "EurekaServer deployment finibated!"
        }
    }
}
