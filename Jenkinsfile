pipeline {
  agent any

  environment {
    SONARQUBE = 'SonarQubeServer' // Name of your SonarQube server in Jenkins config
  }

  stages {

    stage('Checkout') {
      steps {
        git url: 'https://github.com/nishy49/first-dep.git', branch: 'main'
      }
    }

    stage('Build Backend') {
      steps {
        dir('backend') {
          sh 'mvn clean install'
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
          sh 'mvn sonar:sonar -f backend/pom.xml'
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
