pipeline {
    agent { label "Jenkins-Agent" }
    tools {
        jdk 'Java17'
        ant '1.10.12'
    }
    environment {
            APP_NAME = "guessinggame"
            RELEASE = "1.0.0"
            DOCKER_USER = "daniel217x"
            DOCKER_PASS = 'dockerhub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
            BRIDGE_POLARIS_APPLICATION_NAME = "WHIPSO"
            BRIDGE_POLARIS_PROJECT_NAME = "Test CI"
            BRIDGE_POLARIS_BRANCH_NAME = "master"

          
    }

    

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
               steps {
                   git branch: 'main', credentialsId: 'github', url: 'https://github.com/DanielIB-py/GuessingGame'
               }
        }

         stage("Limpiar"){
            steps{
                script{
                    sh('ant limpiar')
                }
            }
        }
        
         stage("Compilar"){
            steps{
                script{
                    sh('ant compilar')
                }
            }
        }
              
   stage('polaris') {
        steps {
            withCredentials([string(credentialsId: 'poc.polarissynopsys.com', variable: 'BRIDGE_POLARIS_ACCESSTOKEN')]) {
                script {
                  
                      sh('curl -fLsS -o bridge.zip $BRIDGECLI_LINUX64 && unzip $RUNNER_TEMP bridge.zip && rm -f bridge.zip && /home/agent/workspace/GuessingGame/synopsys-bridge --verbose --stage polaris polaris.assessment.types=SAST,SCA')
                  
             
                }
            }
        }
    }
                  
         
     
    }
}
