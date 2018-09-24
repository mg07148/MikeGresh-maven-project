pipeline {
    agent any

    tools {
        maven 'Default'
    }

    parameters {
        string(name: 'tomcat_dev',defaultValue: '13.58.217.59', description: 'Staging Server')
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        bat "scp -i /home/Development/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}