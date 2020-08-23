pipeline {
    agent any
    environment {
        PROJECT_ID = 'extreme-minutia-284006'
        CLUSTER_NAME = 'demo'
        LOCATION = 'us-central1-a	'
    }
    stages {
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("shreyas1717/kube:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'shreyas1717') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/Kube:*/Kube:${env.BUILD_ID}/g' Deployment.yaml"
                sh 'kubectl apply -f Deployment.yaml'
                sh 'kubectl apply -f Service.yaml'
            }
        }
        stage("Get frontend service") {
            steps {
                sh 'kubectl get svc'
                sh 'kubectl get pods'
            }
        }
    }    
}
