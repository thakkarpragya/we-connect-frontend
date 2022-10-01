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
          bat """${scannerHome}/bin/sonar-scanner \
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
        bat 'mkdir -p test-reports'
        bat 'npm install --force'
        bat 'npm run build'
      }
    }
    stage('Unit test') {
      steps {
        bat 'npm run test'
        bat 'echo Unit-Test'
      }
    }
    stage('Integration test') {
      steps {
        bat 'npm run integration-test'
        // sh 'npm run generate-report'
        bat 'echo Integration-Test'
      }
    }
    stage('Deploy to staging') {
      steps {
        bat 'rm -rf /Users/nvallore/Desktop/apache-tomcat-10.0.26-staging/webapps/we-connect-frontend/*'
        bat 'scp -r build/* /Users/nvallore/Desktop/apache-tomcat-10.0.26-staging/webapps/we-connect-frontend/'
      }
    }
        stage('Deploy to production') {
      steps {
        input message: 'Push to prod? (Click "Proceed" to continue)'
        bat 'rm -rf /Users/nvallore/Desktop/apache-tomcat-10.0.26-production/webapps/we-connect-frontend/*'
        bat 'scp -r build/* /Users/nvallore/Desktop/apache-tomcat-10.0.26-production/webapps/we-connect-frontend/'
      }
    }
  }
}
