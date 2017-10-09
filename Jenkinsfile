pipeline {
    agent {
        docker { image 'emtrout/dind:latest' }
    }
    stages {
        stage('EI Test') {
            steps {
                sh '''
                    source /docker-lib.sh
                    start_docker
                    docker images

                   '''
            }
        }
    }
   }