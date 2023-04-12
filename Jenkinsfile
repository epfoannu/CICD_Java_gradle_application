pipeline {
    agent {
        docker {
            image 'openjdk:11'
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemonargs '--user root -v /var/run/docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemon
        }
    }
    stages {
        stage('Checkout') {
            steps {
                sh 'echo passed'
                //git branch: 'devops', url: 'https://github.com/epfoannu/CICD_Java_gradle_application.git'
      }
    }
    }
}

      

  
