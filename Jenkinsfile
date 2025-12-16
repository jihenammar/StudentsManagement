pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build + Tests') {
            steps {
                sh 'mvn clean verify'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    try {
                        withSonarQubeEnv('SonarQube') {
                            sh '''
                              mvn sonar:sonar \
                                -Dsonar.projectKey=students-management \
                                -Dsonar.projectName=StudentsManagement \
                                -Dsonar.login=$SONAR_AUTH_TOKEN
                            '''
                        }
                    } catch (Exception e) {
                        echo '⚠️ SonarQube indisponible, étape ignorée'
                    }
                }
            }
        }
    }
}
