    pipeline {
        agent any
     
        tools {
            maven 'localMaven'
        }
     
    stages{
            stage('Build'){
                steps {
                    sh 'mvn clean package'
                }
                post {
                    success {
                        echo 'Now Archiving...'
                        archiveArtifacts artifacts: '**/target/*.war'
                    }
                }
            }
          stage ('Deploy to Staging'){
		steps {
			build job:'Deploy-to-staging'
		}
	  }
	  stage ('Deploy to Production'){
                steps {
                        build job:'Deploy-to-prod'
                }
          }

        }
    }
