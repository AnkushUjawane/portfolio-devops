pipeline{
    agent any

    environment{
        APP_REPO = "https://github.com/AnkushUjawane/AnkushUjawane.github.io.git"
        DOCKERHUB_REPO = "docker.io/ankush1808/portfolio"
        BUILD_TAG = "${env.BUILD_NUMBER}"
    }

    stages{
        stage('checkout code'){
            steps{
                checkout scm
            }
        }

        stage('Checkout portfolio source repo'){
            steps{
                sh '''
                rm -rf app
                git clone ${APP_REPO} app
                '''
            }
        }

        stage('Build docker image'){
            steps{
                sh '''
                cd app
                docker build -t ${DOCKERHUB_REPO}:${BUILD_TAG} \
                -t ${DOCKERHUB_REPO}:latest .
                '''
            }
        }

        stage('Docker Hub Login'){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]){
                    sh '''
                    echo "$DOCKERHUB_PASS" | docker login -u "$DOCKERHUB_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image to Docker Hub'){
            steps{
                sh ''' 
                docker push ${DOCKERHUB_REPO}:${BUILD_TAG}
                docker push ${DOCKERHUB_REPO}:latest
                '''
            }
        }

        stage('cleanup local images'){
            steps{
                sh '''
                docker rmi ${DOCKERHUB_REPO}:${BUILD_TAG} || true
                docker rmi ${DOCKERHUB_REPO}:latest || true
                '''
            }
        }
    }

    post{
        success{
            echo "docker image pushed successfully with tag ${BUILD_TAG}"
        }
        failure{
            echo "pipeline failed"
        }
    }
}