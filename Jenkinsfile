pipeline {
    agent any
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
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
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
                    withCredentials([string(credentialsId: 'nexus_credentials', variable: 'nexus_pass')]) {
                    sh '''
                    docker build -t 192.168.139.150:8083/springapp:${VERSION} .
                    docker login -u admin -p $nexus_pass 192.168.139.150:8083
                    docker push  192.168.139.150:8083/springapp:${VERSION}
                    docker rmi 192.168.139.150:8083/springapp:${VERSION}
                    '''
                    }
                }
            }
        }
    }
}


  


