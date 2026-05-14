pipeline {
    agent any

    environment {
        CONTAINER_NAME = "nestjs-app"
        IMAGE_NAME = "nestjs-image"
        EMAIL = "tariqareeba280@gmail.com"
        PORT = "3000"
    }

    stages {

        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/tariqareeba280-pixel/nestApp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Stop & Remove Previous Container') {
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                '''
            }
        }

        stage('Docker Container Run') {
            steps {
                sh '''
                    docker run -d -p ${PORT}:${PORT} --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }

    }

    post {
        success {
            echo '✅ Deployment Successful!'
            emailext(
                subject: "✅ NestJS App Deployed Successfully!",
                body: """
                    <h2>Deployment Successful!</h2>
                    <p>Your NestJS app has been deployed successfully.</p>
                    <p><b>App URL:</b> <a href="http://13.53.205.116:${PORT}/">http://13.53.205.116:${PORT}/</a></p>
                    <p><b>Build No:</b> #${BUILD_NUMBER}</p>
                    <p><b>Branch:</b> main</p>
                """,
                to: "${EMAIL}",
                mimeType: 'text/html'
            )
        }
        failure {
            echo '❌ Deployment Failed!'
            emailext(
                subject: "❌ NestJS Build/Deployment FAILED!",
                body: """
                    <h2>Deployment Failed!</h2>
                    <p>Something went wrong during the build or deployment.</p>
                    <p><b>Build No:</b> #${BUILD_NUMBER}</p>
                    <p><b>Branch:</b> main</p>
                    <p>Please check Jenkins logs for details.</p>
                """,
                to: "${EMAIL}",
                mimeType: 'text/html'
            )
        }
    }
}