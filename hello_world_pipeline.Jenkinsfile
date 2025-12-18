pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "idan2025/hello-world-app"
        DOCKER_TAG   = "latest"
    }

    options {
        // ansiColor('xterm')  // REMOVE THIS LINE
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Cloning repo...'
                git branch: 'main',
                    url: 'https://github.com/idan2025/final_project_1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image ${DOCKER_IMAGE}:${DOCKER_TAG}"
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', 
                                                  usernameVariable: 'DOCKER_USER', 
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Kubernetes...'
                sh "kubectl apply -f k8s/deployment.yaml"
                sh "kubectl apply -f k8s/service.yaml"
                
                // Wait for rollout to complete
                sh "kubectl rollout status deployment/hello-world-deployment"
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD pipeline completed successfully!'
            sh 'kubectl get pods'
            sh 'kubectl get service hello-world-service'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
        always {
            sh 'docker logout'
        }
    }
}