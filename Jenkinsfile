pipeline{
    agent any
    stages{
        stage('checkout'){
            steps{
                git branch: 'main',
                url: 'https://github.com/AnkushUjawane/portfolio-devops.git'
            }
        }

        stage('Build docker image'){
            steps{
                sh '''
                docker build -t portfolio-devops:latest .
                '''
            }
        }
        stage('Run Container'){
            steps{
                sh '''
                docker stop portfolio-devops || true
                docker rm portfolio-devops || true
                docker run -d -p 3001:80 --name devops-porfolio portfolio-devops:latest
                '''
            }
        }
    }
}