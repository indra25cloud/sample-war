pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'Java17'
    }

    environment {
        TOMCAT_HOST = 'ec2-user@13.235.243.91'
        TOMCAT_PATH = '/opt/tomcat9/webapps'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/indra25cloud/sample-war.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy WAR') {
            steps {
                sshagent (credentials: ['tomcat-ssh-key']) {
                    sh """
                        scp target/sample.war ${TOMCAT_HOST}:${TOMCAT_PATH}/
                        # Tomcat auto-deploys WAR files placed in webapps, no restart needed
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}
