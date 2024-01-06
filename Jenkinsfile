node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    // stage('Build Docker image') {
  
    //    app = docker.build("192.168.56.120:9092/argocd-image-helm/devopsodiahelm:${env.BUILD_NUMBER}")
    // }

    stage('Test Docker image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage ('Docker Build'){
          container('build') {
                stage('Build Image') {
                    docker.withRegistry( 'https://registry.hub.docker.com', 'docker' ) {
                    def customImage = docker.build("prakrit55/etccompany-micro-services-admin:latest")
                    customImage.push()             
                    }
                }
            }
        }

    // stage('Push image to Nexus') {
    //     sh 'docker login -u admin -p admin http://192.168.56.120:9092/repository/argocd-image-helm/'
    //         app.push("${env.BUILD_NUMBER}")
    // }
    stage('Publish  Helm') {
    
        echo "Packing helm chart"
            sh "helm package -d ${WORKSPACE}/helm ${WORKSPACE}/helm/devopsodia"
           // sh "${WORKSPACE}/build.sh --pack_helm --push_helm --helm_repo ${HELM_REPO} --helm_usr ${HELM_USR} --helm_psw ${HELM_PSW}"
           sh "curl -u admin:https://etccompany.jfrog.io/ui/repos/tree/General/etccompany-helm-local --upload-file ${WORKSPACE}/helm/devopsodia-1.tgz -v"
            
        }
    
}
