pipeline {
    agent any
	
	tools{
		maven 'localMaven'
	}

    parameters {
         string(name: 'tomcat_stg', defaultValue: 'http://localhost:8090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'http://localhost:8091', description: 'Production Server')
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
                        bat "winscp **/*.war ${params.tomcat_stg}:/apache-tomcat-8.5.37/apache-tomcat-8.5.37 staging/webapps"
                    }
                }
            }
        }
    }
}