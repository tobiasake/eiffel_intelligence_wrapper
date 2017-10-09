podTemplate(label: 'my-build-pod', containers: [
    // This is the official jnlp base image with added support for ssh access to git repos
    containerTemplate(name: 'jnlp', image: 'larribas/jenkins-jnlp-slave-with-ssh:1.0.0', args: '${computer.jnlpmac} ${computer.name}'),
    // This image gives us a container with kubectl installed so we can deploy the image we build
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl', ttyEnabled: true, command: 'cat'),
    // A docker container to build our image
    containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true)]) {
    node ('my-build-pod') {
        // Store ssh credentials in Jenkins, replace GITHUB_URL with your Github Repo
        git branch: "${env.BRANCH_NAME}", credentialsId: "{{JENKINS_CREDENTIALS}}", url: '{{GITHUB_URL}}'

        stage 'Build docker image'
        container('docker') {
            sh """
                docker build -t ${imageTag} .
            """
        }

        stage 'Push image to the registry'
        container('docker') {
            // Store these credentials in Jenkins too
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: '{{DOCKER_CREDENTIALS}}', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                sh """
                    docker login -u $USERNAME -p $PASSWORD
                    docker push ${imageTag}
                """
            }
        }

        stage 'Deploy app'
        container('kubectl') {
        // This uses a kubeconfig file stored as a Jenkins secret
        withCredentials([[
                $class: 'FileBinding',
                credentialsId: 'k8s-credentials',
                variable: 'KUBECONFIG'
            ]]){
            switch (env.BRANCH_NAME)
            {
                case "master":
                    sh("kubectl --kubeconfig=$KUBECONFIG --namespace={{APP_NAMESPACE}} apply -f ./services.yaml")
                    sh("kubectl --kubeconfig=$KUBECONFIG --namespace={{APP_NAMESPACE}} apply -f ./deployment.yaml")
                    break
            }
        }
    }
  }
}