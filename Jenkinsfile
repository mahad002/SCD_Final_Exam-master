pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerHub')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                git 'https://github.com/mahad002/SCD_Final_Exam-master.git'
            }
        }

        stage('Build and Push Docker Images') {
            steps {
                script {
                    // Log in to Docker Hub
                    docker.withRegistry('', DOCKER_HUB_CREDENTIALS) {
                        // Build and push each service
                        def services = ['backend-auth', 'backend-classroom', 'frontend-client', 'backend-event-bus', 'backend-post']
                        services.each { service ->
                            sh """
                                docker-compose build ${service}
                                docker-compose push ${service}
                            """
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up any leftover Docker artifacts
            sh 'docker system prune -f'
        }
    }
}
