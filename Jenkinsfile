pipeline {
    environment {
        registry = "mahad002/final"
        registryCredential = 'dockerHub'
        dockerImage = ''
    }
    agent any
    stages {
        stage('1239 Cloning our Git') {
            steps {
                def branchName = 'main'
                git 'https://github.com/mahad002/SCD_Final_Exam-master.git', branch: branchName
            }
        }
        stage('1239 Building our images') {
            steps {
                script {
                    def services = [
                        [path: './Auth', image: 'backend-auth'],
                        [path: './Classrooms', image: 'backend-classroom'],
                        [path: './client', image: 'frontend-client'],
                        [path: './event-bus', image: 'backend-event-bus'],
                        [path: './Post', image: 'backend-post']
                    ]
                    
                    services.each { service ->
                        dockerImage = docker.build "${registry}:${service.image}-${env.BUILD_NUMBER}", service.path
                        dockerImage.push()
                    }
                }
            }
        }
        stage('1239 Cleaning up') {
            steps {
                script {
                    def services = ['backend-auth', 'backend-classroom', 'frontend-client', 'backend-event-bus', 'backend-post']
                    services.each { service ->
                        sh "docker rmi ${registry}:${service}-${env.BUILD_NUMBER}"
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                // Conditional cleanup based on OS
                if (isUnix()) {
                    sh 'docker system prune -f'
                } else {
                    bat 'docker system prune -f'
                }
            }
        }
    }
}
