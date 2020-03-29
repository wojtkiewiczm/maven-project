pipeline {
    agent any
	
	    tools {
            maven 'localMaven'
        }


    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.184.14.167', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.156.6.135', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /home/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:"
                        sh "ssh ec2-user@${params.tomcat_dev}: -i tomcat-demo.pem sudo mv **/target/*.war /var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/Jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
