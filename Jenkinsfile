pipeline {
    agent any

    environment {
        // FIX: makes docker/kubectl available to Jenkins
        PATH = "/usr/local/bin:/usr/bin:/bin:/opt/homebrew/bin:$PATH"

        // your docker image name
        IMAGE_NAME = "hibah123/kubeimage1"
        IMAGE_TAG = "v1"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t kubdemoapp:${IMAGE_TAG} ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                }
            }
        }

        stage('Tag & Push Image') {
            steps {
                sh "docker tag kubdemoapp:${IMAGE_TAG} ${IMAGE_NAME}"
                sh "docker push ${IMAGE_NAME}"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh "kubectl apply -f deployment.yaml --validate=false"
                sh "kubectl apply -f service.yaml"
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESS 🚀 App deployed successfully"
        }
        failure {
            echo "Pipeline FAILED ❌ Check logs"
        }
    }
}