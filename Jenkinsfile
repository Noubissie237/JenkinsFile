pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: 'main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Noubissie237/hospital-management.git']])
            }
        }
        
        stage('Recovery') {
            steps {
                
                
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
    
                    sh '''
                        docker pull noubissie237/microservice_gestion_patients:latest
                        docker pull noubissie237/microservice_gestion_personnel:latest
                        docker pull noubissie237/microservice_gestion_pharmacie:latest
                    '''
                }
            }
        }
        
        stage('Build'){
            steps {
                sh '''
                        docker run -d --name patients noubissie237/microservice_gestion_patients:latest
                        docker run -d --name personnel noubissie237/microservice_gestion_personnel:latest
                        docker run -d --name pharmacie noubissie237/microservice_gestion_pharmacie:latest
                '''
            }
        }
        
        stage('Test'){
            steps {
                sh '''
                        docker exec patients python manage.py test
                        docker exec personnel python manage.py test
                        docker exec pharmacie python manage.py test
                '''
            }
        }
        
        stage('Clear'){
            steps {
                sh '''
                        docker rm -f $(docker ps -qa)
                '''
            }
        }
        
    }
}
