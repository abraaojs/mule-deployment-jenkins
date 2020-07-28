pipeline {
  agent any
  environment {
   BUILD_VERSION = "v${currentBuild.number}.RELEASE"
   tool name: 'M2', type: 'maven'
  }
  stages {
    stage('Git Checkout') {
    try {
        //git credentialsId: 'git-token', url: ''
        checkout scm
    } catch(err) {
        sh "echo error ao fazer checkout!"
    }
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
        //sh 'mvn deploy -DmuleDeploy -Dmule.env=master -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW} -DskipTests'
        sh 'mvn -f pom.xml package deploy  -Dusername=abraaojs -Dpassword=68Nf(qxWL*7W"W]yg"Dr/ -DmuleDeploy'


      }
    }
    stage('Deploy CloudHub') {
      environment {
        ANYPOINT_CREDENTIALS = credentials('anypoint.credentials')
      }
      steps {
        //sh 'mvn deploy -P cloudhub -Dmule.version=3.9.0 -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW}'
        //sh 'mvn deploy -DmuleDeploy -Dmule.env=master -Danypoint.username=${ANYPOINT_CREDENTIALS_USR} -Danypoint.password=${ANYPOINT_CREDENTIALS_PSW} -DskipTests'
        sh 'mvn -f pom.xml package deploy  -Dusername=abraaojs -Dpassword=68Nf(qxWL*7W"W]yg"Dr/ -DmuleDeploy'

      }
    }
  }
}




