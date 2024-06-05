pipeline {
    agent any
    
    environment {
        registry = "YourDockerhubAccount/YourRepository"
        registryCredential = 'dockerHub'
    }
    
    stages {
        stage('Build and Push Docker Images') {
            steps {
                script {
                    def services = [
                        'Auth': 'Auth/dockerfile',
                        'Post': 'Post/dockerfile',
                        'Classrooms': 'Classrooms/dockerfile',
                        'client': 'client/dockerfile',
                        'event-bus': 'event-bus/dockerfile'
                    ]
                    
                    services.each { serviceName, dockerfilePath ->
                        def imageTag = "${registry}:${serviceName}_${BUILD_NUMBER}"
                        
                        // Build the Docker image for the service
                        docker.build(imageTag, "-f ${dockerfilePath} .")
                        
                        // Push the Docker image to Docker Hub
                        docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                            docker.image(imageTag).push()
                        }
                        
                        // Clean up: Remove the local Docker image
                        sh "docker rmi ${imageTag}"
                    }
                }
            }
        }
    }
}
