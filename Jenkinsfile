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

      //stage('Build Mulesoft') {
    //  steps {
    //        sh 'mvn -B -U -e -V clean -DskipTests package'
    //}


    //stage('Deploy Nexus') {
    //  steps {
     //       sh ''
    //}

    //stage('Scan Checkmarx') {
      //  try {
        //    step([$class: 'CxScanBuilder', comment: '', credentialsId: 'checkmarx', excludeFolders: '', excludeOpenSourceFolders: '', exclusionsSetting: 'global', failBuildOnNewResults: false, failBuildOnNewSeverity: 'HIGH', filterPattern: '', fullScanCycle: 10, groupId: '1', highThreshold: 3, includeOpenSourceFolders: '', osaArchiveIncludePatterns: '*.zip, *.war, *.ear, *.tgz', osaInstallBeforeScan: false, password: '{AQAAABAAAAAQarlNj2K1k2jtDTsoWEI6+0PUgy3N3c0cKe8rusYA34o=}', preset: '36', projectName: 'webGoat-checkmarx', sastEnabled: true, serverUrl: 'http://checkmarx.alpargatas.com.br', sourceEncoding: '1', useOwnServerCredentials: true, username: '', vulnerabilityThresholdEnabled: true, vulnerabilityThresholdResult: 'FAILURE', waitForResultsEnabled: true])
        //} catch(err) {
          //  sh "echo error ao fazer Deploy Checkmarx!"
        //}
    //}

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

    stage('Deploy Dev') {
        environment {
        DEPLOY_CREDS = credentials('anypoint.credentials')
        }
        when {
        expression { params.deployTo == 'develop' }
        branch 'develop'
        }
        input 'Do you approve deployment DEV?'
        steps {
        withMaven(options: [findbugsPublisher(), junitPublisher(ignoreAttachments: false)]) {
            sh 'mvn deploy -DmuleDeploy -Dmule.env=dev -Danypoint.platform.client_id=db9702f9e269478c93b5b4b4f40e0871 -Danypoint.platform.client_secret=878aC8A4c38944Abb89181dd42F1dB0f'
        }
      }
    }// deploy Dev

    stage('Deploy QA') {
        environment {
        DEPLOY_CREDS = credentials('anypoint.credentials')
        }
        when {
        expression { params.deployTo == 'qa' }
        branch 'qa'
        }
        input 'Do you approve deployment in QA?'
        steps {
        withMaven(options: [findbugsPublisher(), junitPublisher(ignoreAttachments: false)]) {
            sh 'mvn deploy -DmuleDeploy -Dmule.env=qa -Danypoint.platform.client_id=db9702f9e269478c93b5b4b4f40e0871 -Danypoint.platform.client_secret=878aC8A4c38944Abb89181dd42F1dB0f -DskipTests'
        }
       }
    } // deploy QA

    stage('Deploy Prod') {
        environment {
        DEPLOY_CREDS = credentials('anypoint.credentials')
        }
        when {
        expression { params.deployTo == 'master' }
        branch 'master'
        }
        input 'Do you approve deployment PROD?'
        steps {
        withMaven(options: [findbugsPublisher(), junitPublisher(ignoreAttachments: false)]) {
            sh 'mvn deploy -DmuleDeploy -Dmule.env=prod -Danypoint.platform.client_id=db9702f9e269478c93b5b4b4f40e0871 -Danypoint.platform.client_secret=878aC8A4c38944Abb89181dd42F1dB0f -DskipTests'
        }
       }
    } // deploy PROD
}//  end pipeline