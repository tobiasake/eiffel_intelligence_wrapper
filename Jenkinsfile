pipeline {
  agent { kubernetes { name 'jnlp' } }
  stages {
    stage('EI Test') {
      steps {
        sh 'docker images'
      }
    }
  }
}