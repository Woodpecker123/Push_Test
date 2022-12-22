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
    stage('Docker Login') {
      steps {
        echo 'This stage is to create Docker'
        sh 'docker login sbicr6.azurecr.io --username=sbicr6 --password=TaFJ0Y8dupJNvm1JyUvi=75R71D3A0R7'
     
      }
  }
    stage('Docker Pull') {
      steps {
    
        sh 'docker pull sbicr6.azurecr.io/viya-4-x64_oci_linux_2-docker/sas-orchestration:1.70.4-20220322.1647971763125' 
        sh 'docker tag sbicr6.azurecr.io/viya-4-x64_oci_linux_2-docker/sas-orchestration:1.70.4-20220322.1647971763125 sas-orchestration'
        sh 'cd ./operator_deploy && kubectl -n sasoperator   create secret generic sas-orchestration-secret   --type=kubernetes.io/dockerconfigjson   --from-file=.dockerconfigjson=site-config/image-pull-secret.json'


      }
    }
        stage('Docker Run') {
      steps {
    
        sh 'docker run --rm -v $(pwd):/var/lib/jenkins/workspace/VIYA_Deployment_master/ sas-orchestration  create sas-deployment-cr  --deployment-data /var/lib/jenkins/workspace/VIYA_Deployment_master/license/SASViyaV4_9CNG4B_certs.zip --license /var/lib/jenkins/workspace/VIYA_Deployment_master/license/SASViyaV4_9CNG4B_1_stable_2021.2.6_license_2022-04-22T121053.jwt --user-content /var/lib/jenkins/workspace/VIYA_Deployment_master/viya4/ --cadence-name stable  --cadence-version 2021.2.6  > viya4-sasdeployment.yaml'
        }
    }
         stage('Deploy Pods') {
      steps {
    
        sh 'kubectl -n sasoperator  apply -f viya4-sasdeployment.yaml'

      }
    }
  

  }
  environment {
    KUBECONFIG = '/var/lib/jenkins/kube.config'    
  }
}
