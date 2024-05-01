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
                sh '/home/anchal/Documents/MAVEN/apache-maven-3.9.6/bin/mvn install'
            }
        }
        stage('Deployment') {
            steps {
                script {
                    if (env.ENVIRONMENT == 'QA') {
                        sh 'cp target/Pipeline1.war /home/anchal/Documents/MAVEN/apache-tomcat-9.0.88/webapps'
                        echo "Deployment has been done on QA!"
                    } else if (env.ENVIRONMENT == 'UAT') {
                        sh 'cp target/Pipeline1.war /home/anchal/Documents/MAVEN/apache-tomcat-9.0.88/webapps'
                        echo "Deployment has been done on UAT!"
                    }
                }
            }
        }
        stage('Slack') {
            steps {
                slackSend baseUrl: 'https://hooks.slack.com/services/', channel: '#slack3', color: 'good', message: 'hello this is slack integrate', notifyCommitters: true, teamDomain: 'Devops'
            }
        }
    }
}
