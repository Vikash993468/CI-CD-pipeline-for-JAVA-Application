pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage("sonar quality check"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-password') {
                            sh 'chmod +x gradlew'
                            sh "java -version"
                            sh './gradlew sonarqube'
        
                    }

                     timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
                }
            }
            
        }
        stage("docker build & docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 34.16.171.11:8484/springapp:${VERSION} .
                                docker login -u admin -p $docker_password 34.16.171.11:8484 
                                docker push  34.16.171.11:8484/springapp:${VERSION}
                                docker rmi 34.16.171.11:8484/springapp:${VERSION}
                                '''
                    }
                }
            }
        }

    }
}
