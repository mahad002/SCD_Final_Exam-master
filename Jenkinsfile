pipeline {
    agent any
    
    stages {
        stage('Docker Login') {
            steps {
                script {
                    sh 'docker login -u mahad002 -p p.Mahad08'
                }
            }
        }
        
        stage('1239 Build Docker Images') {
            steps {
                script {
                    docker.build('mahad002/final:backend-auth', './Auth')
                    docker.build('mahad002/final:backend-classroom', './Classrooms')
                    docker.build('mahad002/final:frontend-client', './client')
                    docker.build('mahad002/final:backend-event-bus', './event-bus')
                    docker.build('mahad002/final:backend-post', './Post')
                }
            }
        }
        
        stage('1239 Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '<docker-hub-credentials-id>') {
                        docker.push('mahad002/final:backend-auth')
                        docker.push('mahad002/final:backend-classroom')
                        docker.push('mahad002/final:frontend-client')
                        docker.push('mahad002/final:backend-event-bus')
                        docker.push('mahad002/final:backend-post')
                    }
                }
            }
        }
    }
}
