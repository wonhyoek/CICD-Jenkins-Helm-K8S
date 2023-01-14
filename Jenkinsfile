pipeline{
    agent any

    environment{
        VERSION = "${env.BUILD_ID}"
    }

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

        stage("docker build & docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t nexus:8083/springapp:${VERSION} .
                                docker login -u admin -p $docker_password nexus:8083
                                docker push  nexus:8083/springapp:${VERSION}
                                docker rmi nexus:8083/springapp:${VERSION}
                            '''
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