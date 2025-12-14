pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
        GCP_PROJECT = 'disco-light-479913-j6'
    }

    stages {

        stage("Cloning from Github") {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/satyanarayana8055/MLOPS-RECOMENDED-SYSTEM-2.git']]
                )
            }
        }

        stage("Making a virtual environment") {
            steps {
                sh '''
                    python -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install -e .
                '''
            }
        }

        stage('DVC Pull') {
            steps {
                withCredentials([file(credentialsId:'gcp-key2', variable:'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                        . ${VENV_DIR}/bin/activate
                        dvc pull
                    '''
                }
            }
        }

        stage('Build and Push Image to GCR') {
            steps {
                withCredentials([file(credentialsId:'gcp-key2', variable:'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                        export PATH=/usr/bin:$PATH
                        gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                        gcloud config set project ${GCP_PROJECT}
                        gcloud auth configure-docker --quiet

                        docker build -t gcr.io/${GCP_PROJECT}/mlproject:latest .
                        docker push gcr.io/${GCP_PROJECT}/mlproject:latest
                    '''
                }
            }
        }

        stage('Deploying to Kubernetes') {
            steps {
                withCredentials([file(credentialsId:'gcp-key2', variable:'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                        export PATH=/usr/bin:$PATH
                        gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                        gcloud config set project ${GCP_PROJECT}
                        gcloud container clusters get-credentials ml-app-cluster --region us-central1
                        kubectl apply -f deployment.yaml
                    '''
                }
            }
        }
    }
}
