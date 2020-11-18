pipeline {
    environment {
      PROJECT = "myproject-ahsan-123"
      APP_NAME = "aspnet-microservices-jenkins"
      IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}"
    }
    agent any
    stages {
        stage('Set Project and Zone') {
          steps {
            withCredentials([[$class: 'FileBinding', credentialsId:"gcloud", variable: 'JSON_KEY']]) {
              sh 'gcloud auth activate-service-account --key-file $JSON_KEY'
              sh 'gcloud --version'
              // sh 'gcloud config set project myproject-ahsan-123'
              // sh 'gcloud config set compute/zone us-central1-f'
            }
          }    
        }
        stage('Build and push image with Container Builder') {
          steps {
            sh 'gcloud builds submit -t ${IMAGE_TAG} .'
          }
        }
        stage('Create cluster') {
          steps {
            sh 'gcloud run deploy ${APP_NAME} --image ${IMAGE_TAG} --platform managed --region us-central1 --allow-unauthenticated'
          }
        }
    }
}
