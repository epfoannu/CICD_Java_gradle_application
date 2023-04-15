pipeline {
    agent {
        docker {
            image 'openjdk:11'
            args '--user root -v /var/run/docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemonargs '--user root -v /var/run/docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemon
        }
    }
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages {
        stage('Checkout') {
            steps {
                sh 'echo passed'
                //git branch: 'devops', url: 'https://github.com/epfoannu/CICD_Java_gradle_application.git'
            }
        }
        stage('sonar quality status'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar_token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                }
            }
        }
        stage('Quality Gate Status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar_token'
                }
            }
        }
        stage('docker build & docker push to nexus repo'){
            steps{
                script{
                    nexusArtifactUploader credentialsId: 'in.annu', groupId: 'in.annu', nexusUrl: '192.168.139.150', nexusVersion: 'nexus3', protocol: 'http', repository: 'docker-hosted', version: 'snapshot-1.0'
                    
                        
                    }
                }
            }
        }
    }

  


