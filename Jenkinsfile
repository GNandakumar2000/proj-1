pipeline {
    agent any

    environment {
        IMAGE_NAME = 'nandakumar007/taskapp:latest' // Docker Hub image
        KUBE_CONFIG_CREDENTIALS = 'nank8.yaml' // Kubernetes credentials ID
        DEPLOYMENT_NAME = 'taskapp-deployment' // Kubernetes deployment name
        NAMESPACE = 'default' // Kubernetes namespace
    }

    stages {
        stage('Pull Docker Image') {
            steps {
                script {
                    echo 'Using Docker image from Docker Hub: ${IMAGE_NAME}'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Use Kubernetes credentials
                    withCredentials([file(credentialsId: "${KUBE_CONFIG_CREDENTIALS}", variable: 'KUBECONFIG')]) {
                        // Apply the deployment
                        sh """
                        echo 'Deploying to Kubernetes...'
                        kubectl --kubeconfig=\$KUBECONFIG set image deployment/${DEPLOYMENT_NAME} taskapp-container=${IMAGE_NAME} -n ${NAMESPACE}
                        kubectl --kubeconfig=\$KUBECONFIG rollout status deployment/${DEPLOYMENT_NAME} -n ${NAMESPACE}
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after the job is complete
        }
    }
}
