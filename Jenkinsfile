node {

    checkout scm

    env.DOCKER_API_VERSION="1.23"
    
    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-kenzan"
    dockerHubUser = "chihaiaalex"
    imageName = "${dockerHubUser}/${appName}:${tag}"
    env.BUILDIMG=imageName
    registryCredentials = 'dockerhub'
    dockerImage = ''

    stage "Build"

        dockerImage = docker.build -t + "${imageName}" + -f applications/hello-kenzan/Dockerfile applications/hello-kenzan
    
    stage "Push"

        docker.withRegistry('', registryCredentials) {
            dockerImage.push()
        }

    stage "Deploy"

        kubernetesDeploy configs: "applications/${appName}/k8s/*.yaml", kubeconfigId: 'kenzan_kubeconfig'

}
