pipeline {
  agent { kubernetes { jnlp } }
  stages {
    stage('EI Test') {
      steps {
        sh 'docker images'
      }
    }
  }
}