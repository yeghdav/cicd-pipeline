node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Build') {
        sh 'npm install'
    }

    stage('Test') {
        sh 'npm test'
    }

    stage('Build Docker Image') {
        if(env.BRANCH_NAME == 'main'){
            sh 'docker build -t nodemain:v1.0 .'
        }
        else if(env.BRANCH_NAME == 'dev'){
            sh 'docker build -t nodedev:v1.0 .'
        }
    }

    stage('Deploy') {
        sh '''
            docker stop $(docker ps -a -q)
            docker rm $(docker ps -a -q)
        '''
        if(env.BRANCH_NAME == 'main'){
            sh 'docker run -d -p 3000:3000 nodemain:v1.0'
        }
        else if(env.BRANCH_NAME == 'dev'){
            sh 'docker run -d -p 3001:3000 nodedev:v1.0'
        }
    }
}

