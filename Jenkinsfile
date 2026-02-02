pipeline {
    agent any    
    environment {
        DOCKER_IMAGE = 'renderman/dotnet'
        DOCKER_TAG   = "${BUILD_NUMBER}"
        // DOCKER_TAG   = '12'
    }
    stages {
        stage('Build & Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-renderman',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                      docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} .
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }
    } // end of stages
    post {
        always {
            sh "docker logout || true"
        }
        success {
            echo '✅ Build and Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed!'
        }
    }
}
