pipeline {
  agent {
    docker {
      image 'emtrout/dind'
      args 'sh source /docker-lib.sh'
    }
    
  }
  stages {
    stage('') {
      steps {
        sh '''source /docker-lib.sh
start_docker

# Inject K8S config in cmd or in .kube folder. fabric8 plugin requires config
mkdir /root/.kube
cp code/pipeline/config_minikube_included_certs /root/.kube/config

# DockerHub Login
docker login -u {{docker_user}} -p {{docker_password}}

# Compile and run unit tests
# Use TAR from compile step, build Image with TAR and push (both build version and latest pushed)
# Deploy to K8S
mvn -f code/pom.xml clean package fabric8:build fabric8:push fabric8:apply'''
      }
    }
  }
}