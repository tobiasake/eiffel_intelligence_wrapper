pipeline {
agent { none }
node {
        checkout scm
        docker.image('emtrout/dind:latest').inside('-u root') {
            sh 'source /docker-lib.sh'
            sh 'start_docker'
            sh 'docker images'
        }
    }
}