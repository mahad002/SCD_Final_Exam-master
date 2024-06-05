pipeline {
    agent any
    
    environment {
        githubRepo = "https://github.com/mahad002/SCD_Final_Exam-master.git"
        branchName = "main" 
        registry = "mahad002/final"
        registryCredential = 'dockerHub'
    }
    
    stages {
        stage('1239 Clone GitHub Repository') {
            steps {
                git branch: branchName, url: githubRepo
            }
        }
        
        stage('1239 Build and Push Docker Images') {
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
                        def imageTag = "${registry}/${serviceName}:${BUILD_NUMBER}"
                        def dockerfileFullPath = "${workspace}/${dockerfilePath}"
                        
                        // Build the Docker image for the service
                        docker.build(imageTag, "-f ${dockerfileFullPath} ${workspace}")
                        
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
