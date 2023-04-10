pipeline{
    agent any
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
        }
    }








       


    


                

            
    


    
                        

                
    


             
        
        
    




