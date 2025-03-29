pipeline {
    agent any
    
    environment {
        REPO_URL = 'https://github.com/manishbad/node-js-project.git'
        BRANCH = 'main'
        IMAGE_NAME = 'node-app:latest'
        CONTAINER_NAME = 'node-container'
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    retry(3) { // Retry 3 times in case of failure
                        checkout([
                            $class: 'GitSCM', 
                            branches: [[name: "*/${BRANCH}"]],
                            userRemoteConfigs: [[url: REPO_URL]]
                        ])
                    }
                }
            }
        }
        
        
        stage('Docker Build & Run') {
            steps {
                script {
                    sh '''
                    echo "Building Docker Image..."
                    docker build -t ${IMAGE_NAME} .
                    echo "Stopping and Removing existing container (if any)..."
                    docker stop ${CONTAINER_NAME} || true
                    docker rm ${CONTAINER_NAME} || true
                    echo "Running new container..."
                    docker run -d --name ${CONTAINER_NAME} -p 3000:3000 ${IMAGE_NAME}
                    '''
                }
            }
        }
        
    }
    
    post {
        failure {
            echo 'Build failed! Check logs.'
        }
    }
}

