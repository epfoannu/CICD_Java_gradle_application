pipeline{
    agent any
    environment {
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage('sonar quality status'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                    }
                }

                }
            }
        stage('Quality Gate Status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('docker build & docker push to nexus repo'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'nexus_creds')]) {
                        sh '''
                                docker build -t 34.125.214.226:8083/springapp:${VERSION} .
                                docker login -u admin -p $nexus_creds 34.125.214.226:8083 
                                docker push  34.125.214.226:8083/springapp:${VERSION}
                                docker rmi 34.125.214.226:8083/springapp:${VERSION}
                            '''

}
                }
            }
        }
}
}










       


    


                

            
    


    
                        

                
    


             
        
        
    




