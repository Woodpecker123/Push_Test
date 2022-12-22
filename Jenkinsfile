pipeline {
  agent any
  stages {
    stage('Echo') {
      steps {
        writeFile(file: 'Hello.txt', text: 'Hi')
        writeFile(file: 'change.txt', text: 'Hi')
      }
    }
    stage('Operator-Deploy') {
      steps {
        echo 'This is the deployment operator stage'
        sh 'cd ./operator_deploy && kustomize build . | kubectl -n sasoperator apply -f -'        

      }
    }

  }
  environment {
    KUBECONFIG = '/var/lib/jenkins/kube.config'
    
  }
}
