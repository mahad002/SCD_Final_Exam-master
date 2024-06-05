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
                    'auth': 'Auth/dockerfile',
                    'post': 'Post/dockerfile',
                    'classrooms': 'Classrooms/dockerfile',
                    'client': 'client/dockerfile',
                    'event-bus': 'event-bus/dockerfile'
                ]
                
                services.each { serviceName, dockerfilePath ->
                    def imageTag = "${registry}/${serviceName}:${BUILD_NUMBER}"
                    def dockerfileFullPath = "${workspace}/${serviceName}/.dockerfile"

                    // Build the Docker image for the service
                    bat "docker build -t ${imageTag} -f ${dockerfileFullPath} ${workspace}"
            
                    // Push the Docker image to Docker Hub
                    bat "docker push ${imageTag}"
            
                    // Clean up: Remove the local Docker image
                    bat "docker rmi ${imageTag}"
                }
            }
        }
}
    }
}
