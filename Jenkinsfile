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
            def currentService = sh(script: 'microk8s kubectl get service nextjs-service -o json', returnStdout: true).trim()
            def currentDeployment = sh(script: 'microk8s kubectl get deployment nextjs-deployment -o json', returnStdout: true).trim()
            def currentIngress = sh(script: 'microk8s kubectl get ingress nextjs-ingress -o json', returnStdout: true).trim()

            def newService = readFile('nextjs-service.yaml').trim()
            def newDeployment = readFile('nextjs-deployment.yaml').trim()
            def newIngress = readFile('nextjs-ingress.yaml').trim()

            if (currentService != newService || currentDeployment != newDeployment || currentIngress != newIngress) {
                sh "microk8s kubectl apply -f nextjs-service.yaml"
                sh "microk8s kubectl apply -f nextjs-deployment.yaml"
                sh "microk8s kubectl apply -f nextjs-ingress.yaml"
            } else {
                echo 'Service, Deployment, and Ingress configurations are unchanged, skipping deployment'
            }
                
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

