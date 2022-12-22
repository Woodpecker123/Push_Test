pipeline {
  agent any
  stages {
    stage('Echo') {
      steps {
        writeFile(file: 'Hello.txt', text: 'Hi')
        writeFile(file: 'change.txt', text: 'Hi')
      }
    }

  }
}
