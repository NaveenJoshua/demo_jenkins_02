pipeline {
  agent any
  parameters {
    booleanParam(name: 'RC', defaultValue: false, description: 'Is this a Release Candidate?')
  }
  environment {
    VERSION = "0.1.0"        
    VERSION_RC = "rc.2"
    RELEASE = '20.04'
  }
  stages {
    stage('Audit tools') {                        
        steps {
            auditTools()
        }
    }
    stage('Build') {
      environment {
        LOG_LEVEL = 'INFO'
        VERSION_SUFFIX = getVersionSuffix()
      }
      parallel {
        stage('linux-arm64') {
          steps {
            sh '''
                echo "Building release ${VERSION}.${VERSION_SUFFIX} with log level ${LOG_LEVEL}..."
                chmod +x test1.sh 
                ./test1.sh
            '''
          }
        }
        stage('linux-amd64') {
          steps {
            echo "Building release ${RELEASE} for ${STAGE_NAME} with log level ${LOG_LEVEL}..."
          }
        }

        stage('windows-amd64') {
          steps {
            echo "Building release ${RELEASE} for ${STAGE_NAME} with log level ${LOG_LEVEL}..."
          }
        }
      }
    }

    stage('Test') {
      steps {
          echo "Testing Release $RELEASE"
          writeFile file: 'helloResults.txt', text : 'passed'
      }
    }

    stage('Deploy') {
      input {
        message 'Deploy?'
        id 'Do it!'
        parameters {
          string(name: 'TARGET_ENVIRONMENT', defaultValue: 'PROD', description: 'Target deployment environment')
        }
      }
      steps {
        echo "Deploying release ${RELEASE} to environment ${TARGET_ENVIRONMENT}"
      }
    }

  }
  post {
    always {
      echo 'Prints whether deploy happened or not, success or failure'
    }
    success {
      archiveArtifacts 'helloResults.txt'
    }
  }
}
String getVersionSuffix() {
    if (params.RC) {
        return env.VERSION_RC
    } else {
        return env.VERSION_RC + '+ci.' + env.BUILD_NUMBER
    }
}

void auditTools() {
    sh '''
        git version
        docker version
        dotnet --list-sdks
        dotnet --list-runtimes
    '''
}