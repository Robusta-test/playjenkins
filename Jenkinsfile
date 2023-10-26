pipeline {


  agent {
    kubernetes {
      yamlFile 'builder.yaml'
    }
  }

  stages {

    stage('Kaniko Build & Push Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/Dockerfile \
                             --context `pwd` \
                             --destination=ahmedgmansour/myweb:${BUILD_NUMBER}
            '''
          }
        }
      }
    }

    stage('Deploy App to Kubernetes') {     
      steps {
        container('kubectl') {
          script {
            kubernetesDeploy(
              configs: 'myweb.yaml'
              kubeconfigId: 'mykubeconfig'
            )
          // withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
          //   sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" myweb.yaml'
          //   sh 'kubectl apply -f myweb.yaml'
          }
        }
      }
    }
  
  }
}
