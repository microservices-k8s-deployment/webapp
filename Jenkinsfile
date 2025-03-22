pipeline {
   agent any

   environment {     
     SERVICE_NAME = "service_name"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      stage('Preparation') {
         steps {
            cleanWs()
            git credentialsId: 'GitHub', url: "https://github.com/${ORGANIZATION_NAME}/${SERVICE_NAME}"
         }
      }
      stage('Build') {
         steps {
            sh 'echo No build required for service_name.'
         }
      }

      stage('Build and Push Image') {
         steps {
            withDockerRegistry([credentialsId: 'docker-hub-credentials', url: '']) {
                sh 'docker image build -t ${REPOSITORY_TAG} .'
                sh 'docker push ${REPOSITORY_TAG}'
            }
         }
      }

      stage('Deploy to Cluster') {
          steps {
            sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}

