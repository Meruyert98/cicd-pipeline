pipeline {
    agent {
        docker {
            image 'node:lts'
        }
    }
    stages {
        stage('Checkout Repository') {
            steps {
                echo 'Checking out the repository...'
                checkout scm
            }
        }
        stage('Build Application') {
            steps {
                echo 'Building the application...'
                sh 'chmod +x scripts/build.sh'
                sh './scripts/build.sh'
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                sh 'chmod +x scripts/test.sh'
                sh './scripts/test.sh'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Building the Docker image...'
                script {
                    docker.build('meruyerts/mybuildimage')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                echo 'Pushing the Docker image to the registry...'
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_creds_id') {
                        def app = docker.image('meruyerts/mybuildimage')
                        app.push("${env.BUILD_NUMBER}")
                        app.push('latest')
                    }
                }
            }
        }
    }
}
