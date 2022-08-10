pipeline {
    agent any
    
    tools {
        maven 'local_maven'
    }
    parameters {
         string(name: 'staging_server', defaultValue: '13.126.48.59', description: 'Remote Staging Server')
    }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ("Deploy to Staging"){
                    steps {
                        sshagent(['b0962c0e-e3d6-4ff1-8cca-7a282bad7546']) {
                        sh "scp **/*.war jenkins@${params.tomcat_stag}:/opt/tomcat/webapps"
      }
                    }
                }
            }
        }
    }
}
