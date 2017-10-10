podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'docker', image: 'docker', ttyEnabled: true, command: 'cat'),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.0', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]) {
    node('mypod') {

        stage('do some Docker work') {
            container('docker') {

                withCredentials([[$class: 'UsernamePasswordMultiBinding',
                        credentialsId: 'e7de4146-4a59-4406-916e-d10506cfaeb8',
                        usernameVariable: 'DOCKER_HUB_USER',
                        passwordVariable: 'DOCKER_HUB_PASSWORD']]) {

                    sh """
                        docker pull ubuntu
                        docker tag ubuntu ${env.DOCKER_HUB_USER}/ubuntu:${env.BUILD_NUMBER}
                        """
                    sh "docker login -u ${env.DOCKER_HUB_USER} -p ${env.DOCKER_HUB_PASSWORD} "
                    sh "docker push ${env.DOCKER_HUB_USER}/ubuntu:${env.BUILD_NUMBER} "
                }
            }
        }

        stage('do some kubectl work') {
            container('kubectl') {
               withCredentials([[
                $class: 'FileBinding',
                credentialsId: '77bd756d-382a-4704-bb9c-9d52f023ac4d',
                variable: 'KUBECONFIG'
            ]]){
                 sh("kubectl --kubeconfig=$KUBECONFIG --namespace={{APP_NAMESPACE}} get nodes")
               }
            }
        }

        stage('do some helm work') {
            container('helm') {
            environment {
                            KUBECONFIG = $KUBECONFIG
                        }
            withCredentials([[
                $class: 'FileBinding',
                credentialsId: '77bd756d-382a-4704-bb9c-9d52f023ac4d',
                variable: 'KUBECONFIG'
            ]]){
                sh "export KUBECONFIG=$KUBECONFIG"
                sh "helm ls"
               }
            }

        }

    }
}