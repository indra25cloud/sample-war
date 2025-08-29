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
                sh 'mvn clean package'
            }
        }

        stage('Deploy WAR') {
            steps {
                sh """
                scp target/sample-war.war ${TOMCAT_HOST}:${TOMCAT_PATH}/
                ssh ${TOMCAT_HOST} '/opt/tomcat9/bin/shutdown.sh || true'
                ssh ${TOMCAT_HOST} '/opt/tomcat9/bin/startup.sh'
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
