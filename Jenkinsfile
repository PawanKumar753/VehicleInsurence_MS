pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "eurekaserver:latest"
        APP_NAME = "EurekaServer"
        WORKSPACE_DIR = "${env.WORKSPACE}\\EurekaServer"
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code..."
                git url: 'https://github.com/PawanKumar753/VehicleInsurence_MS.git', branch: 'master', credentialsId: 'springboot-jenkins4'
            }
        }

        stage('Build JAR') {
            steps {
                echo "Building JAR for ${APP_NAME}..."
                bat "mvn -f ${WORKSPACE_DIR}\\pom.xml clean package -DskipTests"
            }
        }

        stage('Check Environment') {
            steps {
                bat "echo Jenkins Home: %JENKINS_HOME%"
                bat "whoami"
                bat "docker --version"
                bat "docker ps"
            }
        }


        stage('Build Docker Image') {
            steps {
                echo "Building Docker image: ${DOCKER_IMAGE}..."
                dir(WORKSPACE_DIR) {
                    bat "dir"
                    // Ensure Dockerfile exists
                    bat "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

      stage('Run Docker Container') {
            steps {
                echo "Running Docker container for ${APP_NAME}..."
                bat "docker stop ${APP_NAME} || exit 0"
                bat "docker rm ${APP_NAME} || exit 0"
                bat "docker run -d -p 8761:8761 --name ${APP_NAME} ${DOCKER_IMAGE}"
            }
        }




    }

    post {
        success {
            echo "${APP_NAME} deployed successfully!"
        }
        failure {
            echo "Deployment failed for ${APP_NAME}!"
        }
    }
}
