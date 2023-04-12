pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        sh 'echo passed'
        checkout scmGit(branches: [[name: '*/devops']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/epfoannu/CICD_Java_gradle_application.git']])'
        
        
      }
    }
}
}

      

  
