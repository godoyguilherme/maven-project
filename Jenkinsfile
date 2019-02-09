pipeline {
    agent any
	
	tools{
		maven 'localMaven'
	}

    parameters {
         string(name: 'tomcat_stg', defaultValue: '18.231.192.24', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.228.235.163', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "winscp -i /Users/GGDY/Desktop/tomcat-demo.pem **/*.war ec2-user@${params.tomcat_stg}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i /Users/GGDY/Desktop/tomcat-demo.pem **/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}