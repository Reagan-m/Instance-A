pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                credentialsId: 'github-credentials',
                url: 'https://github.com/Reagan-m/frontend.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t frontend-app .
                '''
            }
        }

        stage('Stop Old Container') {
            steps {
                sh '''
                OLD_CONTAINER=$(docker ps -aq -f name=frontend-container)
                if [ ! -z "$OLD_CONTAINER" ]; then
                    echo "Removing old container..."
                    docker rm -f $OLD_CONTAINER
                fi
                '''
            }
        }

        stage('Run New Container') {
            steps {
                sh '''
                docker run -d --name frontend-container -p 3000:3000 frontend-app
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}

