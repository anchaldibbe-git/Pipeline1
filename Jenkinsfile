pipeline {
    agent any
    
    parameters {
        choice(name: 'ENVIRONMENT', choices: ['QA', 'UAT'], description: 'Pick Environment value')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm			       
            }
        }
        stage('Build') {
            steps {
                script {
                    try {
                        sh '/home/anchal/Documents/MAVEN/apache-maven-3.9.6/bin/mvn clean install'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Build failed: ${e.message}")
                    }
                }
            }
        }
        stage('Deployment') {
            steps {
                script {
                    def destination = env.ENVIRONMENT == 'QA' ? '/home/anchal/Documents/MAVEN/apache-tomcat-9.0.88/webapps/QA' : '/home/anchal/Documents/MAVEN/apache-tomcat-9.0.88/webapps/UAT'
                    sh "cp target/Pipeline1.war $destination"
                    echo "Deployment has been done on ${env.ENVIRONMENT}!"
                }
            }
        }
        stage('Slack Notification') {
            steps {
                script {
                    slackSend baseUrl: 'https://hooks.slack.com/services/', channel: '#slack4', color: 'good', message: 'this is slack integration', notifyCommitters: true, teamDomain: 'Devops'
                }
            }
        }
    }
}
