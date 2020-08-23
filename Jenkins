pipeline {
    agent any
    environment {
        PROJECT_ID = 'summer-sun-286703'
        CLUSTER_NAME = 'kube-demo'
        LOCATION = 'asia-southeast1-a'
    }
    stages {
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("vishnusk/kube-demo:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerid') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/kube-demo:*/kube-demo:${env.BUILD_ID}/g' Deployment.yaml"
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
