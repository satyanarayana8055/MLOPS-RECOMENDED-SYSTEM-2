pipeline {
     agent any 

     environment {
          VENV_DIR = 'venv'
     }

     stages{
          stage("Cloning from Github..."){
               steps{
                    script{
                         echo 'Cloing from Github...'
                         checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/satyanarayana8055/MLOPS-RECOMENDED-SYSTEM-2.git']])
                    }
               }
          }
          stage("Making a virtual environment..."){
               steps{
                    script{
                         echo 'Making a virtual environment...'
                         sh '''
                         python -m venv ${VENV_DIR}
                         . ${VENV_DIR}/bin/activate
                         pip install --upgrade pip
                         pip install -e .
                         '''
                    }
               }
          }

          stage('DVC Pull'){
               step{
                    withCredentials([file(credentialsId:'gcp-key2', variable:'GOOGLE_APPLICATION_CREDENTIALS')]){
                         script{
                              echo 'DVC Pull...'
                              sh'''
                              . ${VENV_DIR}/bin/activate
                              dvc pull
                              '''
                         }
                    }
               }
          }
     }
}
