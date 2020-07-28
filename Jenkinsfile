pipeline {
  agent any
  environment {
   tool name: 'M2', type: 'maven'
  }
  stages {
    stage('Unit Test') {
      steps {
        sh 'mvn clean test'
      }
    }
    stage('Deploy Standalone') {
      steps {
        sh 'mvn deploy -P standalone'
      }
    }
    stage('Deploy ARM') {
      environment {
        withCredentials([usernameColonPassword(credentialsId: 'anypoint.credentials', variable: 'mulesoft')]) {
          ANYPOINT_CREDENTIALS = credentials('anypoint.credentials')
        }
      }
      steps {
        //sh 'mvn deploy -P arm -Darm.target.name=local-3.9.0-ee -Danypoint.username=${ANYPOINT_CREDENTIALS_USR}  -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW}'
        sh 'mvn deploy -DmuleDeploy -Dmule.env=master -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW} -DskipTests'

      }
    }
    stage('Deploy CloudHub') {
      environment {
        ANYPOINT_CREDENTIALS = credentials('anypoint.credentials')
      }
      steps {
        //sh 'mvn deploy -P cloudhub -Dmule.version=3.9.0 -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW}'
        sh 'mvn deploy -DmuleDeploy -Dmule.env=master -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW} -DskipTests'

      }
    }
  }
}




