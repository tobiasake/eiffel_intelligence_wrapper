pipeline {
    agent {
        docker { image 'emtrout/dind:latest' }
    }
    stages {
        stage('Test') {
            steps {
                sh '''
                    source /docker-lib.sh
                    start_docker

                    mvn -f pom.xml clean package fabric8:build fabric8:push
                   '''
            }
        }
    }
   }