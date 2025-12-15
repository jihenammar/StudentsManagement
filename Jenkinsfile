pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "jihenbenammar/devops"
    DOCKER_CRED  = "dockercreds"
  }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build & Test') {
      steps {
        sh 'mvn clean verify'
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh '''
            mvn sonar:sonar \
              -Dsonar.projectKey=students-management \
              -Dsonar.projectName=StudentsManagement \
              -Dsonar.host.url=http://192.168.33.10:9000
          '''
        }
      }
    }

    stage('Docker Build') {
      steps {
        script {
          def tag = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
          sh "docker build -t ${DOCKER_IMAGE}:${tag} ."
          sh "docker tag ${DOCKER_IMAGE}:${tag} ${DOCKER_IMAGE}:latest"
        }
      }
    }

    stage('Docker Push') {
      steps {
        withCredentials([
          usernamePassword(
            credentialsId: env.DOCKER_CRED,
            usernameVariable: 'DH_USER',
            passwordVariable: 'DH_PASS'
          )
        ]) {
          sh '''
            echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
            TAG=$(git rev-parse --short HEAD)

            docker push ${DOCKER_IMAGE}:$TAG
            docker push ${DOCKER_IMAGE}:latest

            docker logout
          '''
        }
      }
    }
  }

  post {
    success { echo "✅ Pipeline completed successfully" }
    failure { echo "❌ Pipeline failed" }
  }
}
