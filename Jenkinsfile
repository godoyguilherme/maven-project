pipeline {
    agent any
	
	tools{
		maven 'localMaven'
	}

    parameters {
         string(name: 'tomcat_stg', defaultValue: 'localhost:8090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:8091', description: 'Production Server')
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
                        bat "wincp -i /Users/GGDY/Desktop/tomcat-demo.pem **/*.war ${params.tomcat_stg}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "wincp -i /Users/GGDY/Desktop/tomcat-demo.pem **/*.war ${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}