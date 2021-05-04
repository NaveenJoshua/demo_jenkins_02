pipeline {
  agent any
  stages {
    stage('Build') {
      environment {
        LOG_LEVEL = 'INFO'
      }
      parallel {
        stage('linux-arm64') {
          steps {
            sh '''
                            echo "Building release ${RELEASE} for ${STAGE_NAME} with log level ${LOG_LEVEL}..."
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
        sh '''
                    echo "Testing release ${RELEASE}..."
                    chmod +x test1.sh 
                    ./test1.sh
                '''
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
  environment {
    RELEASE = '20.04'
  }
  post {
    always {
      echo 'Prints whether deploy happened or not, success or failure'
    }

  }
}