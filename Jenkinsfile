pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sshagent(['tomcat_ssh']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no target/hello-tomcat-1.0.0.war ubuntu@<TOMCAT-IP>:/var/lib/tomcat10/webapps/hello-tomcat.war
                        ssh ubuntu@<TOMCAT-IP> "sudo systemctl restart tomcat10"
                    '''
                }
            }
        }
    }
}
