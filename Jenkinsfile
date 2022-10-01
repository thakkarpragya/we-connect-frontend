pipeline {
  agent any
  tools {
    nodejs "NodeJS"
  }
  stages {
    stage('SonarQube analysis') {
      steps {
        script {
          scannerHome = tool 'sonarqube'
        }
        withSonarQubeEnv(installationName: 'sonarqube') {
          bash """${scannerHome}/bin/sonar-scanner \
              -Dsonar.projectKey=we-connect \
               -Dsonar.sources=. \
               -Dsonar.projectName=we-connect \
               -Dsonar.login=sonaradmin \
               -Dsonar.password=sonaradmin \
               -Dsonar.projectVersion=1.0 """
        }
      }
    }
    stage('Build artifacts') {
      steps {
        bash 'mkdir -p test-reports'
        bash 'npm install --force'
        bash 'npm run build'
      }
    }
    stage('Unit test') {
      steps {
        bash 'npm run test'
        bash 'echo Unit-Test'
      }
    }
    stage('Integration test') {
      steps {
        bash 'npm run integration-test'
        // sh 'npm run generate-report'
        bash 'echo Integration-Test'
      }
    }
    stage('Deploy to staging') {
      steps {
        bash 'rm -rf /Users/nvallore/Desktop/apache-tomcat-10.0.26-staging/webapps/we-connect-frontend/*'
        bash 'scp -r build/* /Users/nvallore/Desktop/apache-tomcat-10.0.26-staging/webapps/we-connect-frontend/'
      }
    }
        stage('Deploy to production') {
      steps {
        input message: 'Push to prod? (Click "Proceed" to continue)'
        bash 'rm -rf /Users/nvallore/Desktop/apache-tomcat-10.0.26-production/webapps/we-connect-frontend/*'
        bash 'scp -r build/* /Users/nvallore/Desktop/apache-tomcat-10.0.26-production/webapps/we-connect-frontend/'
      }
    }
  }
}
