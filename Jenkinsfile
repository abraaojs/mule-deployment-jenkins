pipeline {
  agent any
  tools {
      maven 'M2'
  }
  environment {
      CI = 'true'
      echo "branch: ${env.BRANCH_NAME}"
      BUILD_VERSION = "v${currentBuild.number}.RELEASE"
      DEPLOY_CREDS = credentials('anypoint.credentials')
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
        DEPLOY_CREDS = credentials('anypoint.credentials')
      }
      steps {
      withMaven(options: [findbugsPublisher(), junitPublisher(ignoreAttachments: false)]) {
          sh 'mvn deploy -DmuleDeploy -Dmule.env=dev -Danypoint.platform.client_id=db9702f9e269478c93b5b4b4f40e0871 -Danypoint.platform.client_secret=878aC8A4c38944Abb89181dd42F1dB0f'
      }
     }
    }
    stage('Deploy CloudHub') {
      environment {
        DEPLOY_CREDS = credentials('anypoint.credentials')
      }
      steps {
      withMaven(options: [findbugsPublisher(), junitPublisher(ignoreAttachments: false)]) {
        sh 'mvn deploy -DmuleDeploy -Dmule.env=dev -Danypoint.platform.client_id=db9702f9e269478c93b5b4b4f40e0871 -Danypoint.platform.client_secret=878aC8A4c38944Abb89181dd42F1dB0f'
        }
      }
    }
  }
}