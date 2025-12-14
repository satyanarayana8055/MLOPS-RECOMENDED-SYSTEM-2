pipeline {
     agent any 
     stages{
          stage("Cloning from Github..."){
               steps{
                    script{
                         echo 'Cloing from Github...'
                         checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/satyanarayana8055/MLOPS-RECOMENDED-SYSTEM-2.git']])
                    }
               }
          }
     }
}
