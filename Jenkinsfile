pipeline{
    agent any
    environment{
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
                    withCredentials([string(credentialsId: 'nexus_passwd', variable: 'nexus_creds')]) {
                    sh '''
                    docker build -t 192.168.139.150:8083/springapp:${VERSION} .
                    docker login -u admin -p $nexus_creds 192.168.139.150:8083 
                    docker push  192.168.139.150:8083/springapp:${VERSION}
                    docker rmi 192.168.139.150:8083/springapp:${VERSION}
                    '''
                    }
                }
                }
            }
        stage('Identifying misconfigs using datree in helm charts'){
            steps{
                script{
                    dir('kubernetes/myapp/') {
                        withEnv(['DATREE_TOKEN=7d23b613-241b-444f-92dd-33d9ec2c9d40']) {
                        sh 'helm datree test .'
                }
            }
        stage('Pushing the helm charts to nexus repo'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'nexus_passwd', variable: 'nexus_creds')]) {
                         dir('kubernetes/') {
                                sh '''
                                 helmversion=$( helm show chart myapp | grep version | cut -d: -f 2 | tr -d ' ')
                                 tar -czvf  myapp-${helmversion}.tgz myapp/
                                 curl -u admin:$nexus_creds http://192.168.139.150:8081/repository/helm-repo/ --upload-file myapp-${helmversion}.tgz -v
                                '''
                         }
                    }
                }
            }
        }

      
        post {
            always {
                mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "epfoannu@gmail.com";  
            }
        }
                }
            }
            
		 
	   
       
    
}

    












       


    


                

            
    


    
                        

                
    


             
        
        
    




