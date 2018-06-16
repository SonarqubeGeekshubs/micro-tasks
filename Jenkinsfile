node {
    // Mark the code checkout 'stage'....
    stage 'Checkout'
    checkout scm

    stage('SonarQube analysis') {
      def scannerHome = tool 'Sonar-Scanner';
      withSonarQubeEnv('Sonarqube') {
        sh "${scannerHome} -Dsonar.projectKey=micro-tasks -Dsonar.sources=. -Dsonar.host.url=http://192.168.1.189:9000 -Dsonar.login=7bd5ecce2fbc25d697fc50d8ed65f6dcad33fa74"
      }
    }
}
