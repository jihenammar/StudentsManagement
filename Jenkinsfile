pipeline {
    agent any

    tools {
        maven 'M2_HOME'
    }

    options {
        // Timeout starts after the agent is allocated
        timeout(time: 1, unit: 'MINUTES')
    }

    environment {
        APP_ENV = "DEV"
    }

    stages {

        stage('Code Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/hwafa/atelier-jenkins.git',
                    credentialsId: 'jenkins-example-github-pat'
            }
        }

        stage('Code Build') {
            steps {
                sh 'mvn install -Dmaven.test.skip=true'
            }
        }
    }

    post {
        always {
            echo "======always======"
        }
        success {
            echo "=====pipeline executed successfully====="
        }
        failure {
            echo "======pipeline execution failed======"
        }
    }
}
