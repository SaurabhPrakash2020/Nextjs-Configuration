pipeline {
    agent any
    triggers {
        githubPush()
    }
    stages {
        stage('Build') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'saurabhprakash1890559') {
                        docker.build('saurabhprakash1890559/my-nextjs-app:v1')
                        docker.push('saurabhprakash1890559/my-nextjs-app:v1')
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh "kubectl apply -f nextjs-deployment.yaml"
                    sh "kubectl apply -f nextjs-service.yaml"
                    sh "kubectl apply -f nextjs-ingress.yaml"
                }
            }
        }
    }
}
