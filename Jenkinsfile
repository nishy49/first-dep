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
