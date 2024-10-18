pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub')
        IMAGE_NAME = 'nahhla0220/nginx'
        KUBECONFIG = credentials('kubeconfig')
        NAMESPACE = 'dv'
    }
    
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                git url: 'https://github.com/nahlaosama/final_project/tree/master/k8s', branch: 'efdf6284ac37cf35de8fb87f33fb51b72de27f38'
            }
        }

    

    
        stage('Build') {
            steps {
                script {
                    // Log in to Docker Hub using the credentials
                    sh "echo \$DOCKER_HUB_CREDENTIALS_PSW | docker login -u \$DOCKER_HUB_CREDENTIALS_USR --password-stdin"
                    // Build the Docker image
                    sh 'docker build -t nahhla0220/nginx:v1 .'
                }
            }
        }
      

        stage('Push') {
            steps {
                script {
                    
                    // Push the image to Docker Hub
                    sh 'docker push nahhla0220/nginx:v1'
                    
                    // Remove the image after pushing
                    sh 'docker rmi nahhla0220/nginx:v1'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Use the kubeconfig stored as a Jenkins credential
                        // Set KUBECONFIG environment variable for kubectl
                        sh "export KUBECONFIG=${KUBECONFIG_FILE}"

                        // Apply the Kubernetes deployment and service YAML files
                        sh """
                        kubectl apply -f k8s/frontend-deployment.yml -n ${NAMESPACE}
                        kubectl apply -f k8s/frontend-service.yml    -n ${NAMESPACE}
                        kubectl apply -f k8s/backend-deployment.yml  -n ${NAMESPACE}
                        kubectl apply -f k8s/backend-service.yml     -n ${NAMESPACE} 
                        kubectl apply -f k8s/k8s/daemonset.yaml      -n ${NAMESPACE}
                        kubectl apply -f k8s/backend-service.yml     -n ${NAMESPACE} 
                        """
                    }
                }
            }
    
        }
}
   
        
