pipeline{
    agent any
    stages {
        stage("sonar quality check"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'SonarToken') {
                            sh 'chmod +x gradlew'
                            sh './gradlew sonarqube'
        
                    }
                }
            }
            
        }

    }
}