pipeline {
  agent any

  tools {
    nodejs 'NodeJS'
    maven 'Maven'
  }

  environment {
    SONARQUBE = 'SonarQubeServer'
  }

  stages {

    stage('Checkout') {
      steps {
        echo "Code already checked out by Jenkins SCM"
      }
    }

    stage('Build Backend') {
      steps {
        dir('backend') {
          withMaven(maven: 'Maven') {
            sh 'mvn clean install'
          }
        }
      }
    }

    stage('Build Frontend') {
      steps {
        dir('frontend') {
          sh 'npm install'
          sh 'npm run build'
        }
      }
    }

    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv("${SONARQUBE}") {
          withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
            dir('backend') {
              withMaven(maven: 'Maven') {
                sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
              }
            }
          }
        }
      }
    }

    stage('STAG Deploy') {
      steps {
        echo 'Deploying to STAG...'
      }
    }

    stage('UAT Deploy') {
      steps {
        input message: "Approve deploy to UAT?"
        echo 'Deploying to UAT...'
      }
    }

    stage('PROD Deploy') {
      steps {
        input message: "Approve deploy to PROD?"
        echo 'Deploying to PROD...'
      }
    }
  }
}
