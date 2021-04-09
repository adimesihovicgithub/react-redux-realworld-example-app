pipeline {
      agent {
       docker {
        image 'node:alpine'
        args '-p 3000:3000'
       }
      }
     
     stages {
        stage('Faza_Bild') { 
            steps {
                sh '${WORKSPACE}/build.sh ${BRANCH_NAME} ${BUILD_NUMBER}' 
            }
        }
        stage('Faza_Artifakti'){
            steps{
                withAWS(region:'eu-west-1', credentials:'aws') {
                    s3Upload file:"staging/${BRANCH_NAME}_${BUILD_NUMBER}.tar.gzip", bucket:'adibucketartifacts', path:'staging/'
                    s3Upload file:"production/${BRANCH_NAME}_${BUILD_NUMBER}.tar.gzip", bucket:'adibucketartifacts', path:'production/'
                }
            }
        }
        stage('Faza_Stage'){
            steps{
                withAWS(region:'eu-west-1', credentials:'aws'){
                    s3Upload bucket:'adibucketstaging', includePathPattern:'**/*', workingDir:'staging/build'
                }
            }
        }
    }
}
