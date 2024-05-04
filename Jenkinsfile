pipeline {
    agent any
    triggers {
        githubPush()
    }
    stages {
        stage('Build') {

            environment{
                registryCredential='saurabhprakash1890559'
            }
            steps {
                script {
                    // Build and push Docker image
                    docker.withRegistry('https://registry.hub.docker.com', 'saurabhprakash1890559') {
                        def customImage = docker.build('saurabhprakash1890559/my-nextjs-app:v1')
                        customImage.push('v1')
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Deploy the application to Kubernetes
                    sh "kubectl apply -f nextjs-deployment.yaml"
                    sh "kubectl apply -f nextjs-service.yaml"
                    sh "kubectl apply -f nextjs-ingress.yaml"
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            echo 'Deployment failed'
        }
    }
}

