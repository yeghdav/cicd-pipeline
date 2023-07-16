pipeline {
    agent any
    tools { 
        nodejs 'Node' // Replace 'Node' with the name you've given your Node.js installation in Jenkins Global Tool Configuration
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if(env.BRANCH_NAME == 'main'){
                        sh 'docker build -t nodemain:v1.0 .'
                    }
                    else if(env.BRANCH_NAME == 'dev'){
                        sh 'docker build -t nodedev:v1.0 .'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if(env.BRANCH_NAME == 'main'){
                        sh 'docker stop nodemain || true'
                        sh 'docker rm nodemain || true'
                        sh 'docker run -d --name nodemain -p 3000:3000 nodemain:v1.0'
                    }
                    else if(env.BRANCH_NAME == 'dev'){
                        sh 'docker stop nodedev || true'
                        sh 'docker rm nodedev || true'
                        sh 'docker run -d --name nodedev -p 3001:3000 nodedev:v1.0'
                    }
                }
            }
        }
    }
}
