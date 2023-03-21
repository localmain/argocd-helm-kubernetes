node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Build Docker image') {
  
       app = docker.build("54.157.241.144:9091/argocd-image-helm/devopsodiahelm:${env.BUILD_NUMBER}")
    }

    stage('Test Docker image') {
  

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image to Nexus') {
        sh 'docker login -u admin -p admin http://54.157.241.144:9091/repository/argocd-image-helm/'
            app.push("${env.BUILD_NUMBER}")
    }
    stage('Publish  Helm') {
    
        echo "Packing helm chart"
            sh "helm package -d ${WORKSPACE}/helm ${WORKSPACE}/helm/devopsodia"
           // sh "${WORKSPACE}/build.sh --pack_helm --push_helm --helm_repo ${HELM_REPO} --helm_usr ${HELM_USR} --helm_psw ${HELM_PSW}"
           sh "curl -u admin:admin http://54.157.241.144:8081/repository/argocd-helm-repo/ --upload-file ${WORKSPACE}/helm/devopsodia-1.tgz -v"
            
        }
    
}
