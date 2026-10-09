pipeline {

    // agent windows 1

    agent {
        label 'agent-windows'  
       
    }

    environment {
        DOCKERHUB_USER = "bocardocker"
        IMAGE_NAME     = "backend-employe"
        IMAGE_TAG      = "1.${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG} -t ${DOCKERHUB_USER}/${IMAGE_NAME}:latest ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat """
                            docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                            docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                            docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest
                        """
                }
            }
        }
    // stage docker compose
        // stage('Deploy with Docker Compose') {
        //     steps {
        //         bat "docker-compose up -d --build"
        //     }
        // }
    }

    post {
        success {
            echo "Déploiement Backend Gestion employé réussiiiii !"
        }
        failure {
            echo "Le déploiement a échoué, vérifiez les logs Jenkinssss."
        }
    }
}
