pipeline {
  agent { kubernetes { containerTemplate 'jnlp' } }
  stages {
    stage('EI Test') {
      steps {
        sh 'docker images'
      }
    }
  }
}