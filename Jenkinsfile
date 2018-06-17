#!/usr/bin/env groovy

pipeline{
        agent  any

        tools {
                jdk 'JAVA_8'
                maven 'Maven 3.5.3'
        }

        stages{
               
            stage('Init'){
                steps{
                    echo '***** INIT STAGE  *****'
                }
            }
        
            stage('CodeRepo'){
                steps{                        
                    git branch: 'release-blue', credentialsId: 'Github_ID', url: 'https://github.com/zyang32/hello-world-spring-boot.git'                           
                }
            }

            stage('Build'){
                steps{
                    
                    dir('.'){
                        sh "mvn clean install -DskipTests"
                        sh '''pwd
                        ls -al'''                           
                    }

                }   
            }

            stage('deploy "Blue" via PCF'){
                steps {
                    pushToCloudFoundry(
                        target: 'api.run.pivotal.io',
                        organization: 'DHOrgPilot',
                        cloudSpace: 'blue-green',
                        credentialsId: 'PCF_ID',
                        manifestChoice: [
                            manifestFile: 'manifest.yml'
                        ]
                    )
                }
            }
        }

        post{
            success {
                echo "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
            }
                                                
            failure {
                echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
            }
        }
}

