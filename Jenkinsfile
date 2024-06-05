pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerHub')
        GIT_REPO = 'https://github.com/mahad002/SCD_Final_Exam-master.git'
        GIT_BRANCH = 'main'  // Replace 'main' with the default branch of your repository if it's different
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from the repository
                    checkout([$class: 'GitSCM',
                              branches: [[name: env.GIT_BRANCH]],
                              userRemoteConfigs: [[url: env.GIT_REPO]]
                    ])
                }
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
