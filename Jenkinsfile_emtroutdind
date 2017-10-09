pipeline {
    agent {
        docker { image 'emtrout/dind:latest' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'docker images'
            }
        }
    }
   }