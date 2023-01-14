pipeline{
    agent any
    stages {
        stage("sonarqube static code check"){
            agent{
                /**
                Docker Plugin Setting

                Docker
                Docker Pipeline
                docker-build-step
                 */
                docker{
                    image 'openjdk:11'
                }
            }

             /**
            Sonarqube PLUGIN SETTINGS
            
            SonarQube Scanner
            Sonar Gerrit
            SonarQube Generic Coverage
            sonar Quality Gates
            Quality Gates
             */
            steps{
                script{
                   withSonarQubeEnv(credentialsId: 'sonar-password') {
                       sh 'chmod +x gradlew'
                       sh './gradlew sonarqube'
                    }
                    timeout(5) {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK'){
                            error "pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
	
        }

        
    }

    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}