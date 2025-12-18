pipeline {
    agent any

    stages {
        stage('Test') {
            steps {
                sh 'echo Hello Jenkins'
            }
        }

        stage('Checkout Code') {
            steps {
                echo 'Checking out the code'
                git branch: 'main',
                    url: 'https://github.com/idan2025/final_project_1.git'
            }
        }
    }
}
