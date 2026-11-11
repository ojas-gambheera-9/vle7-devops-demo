pipeline {
  agent any
  environment {
    IMAGE = "vle7-app:${env.BUILD_NUMBER}"
  }
  stages {
    stage('Build') {
      steps { sh 'docker build -t $IMAGE .' }
    }
    stage('Load into Minikube') {
      steps { sh 'minikube image load $IMAGE' }
    }
    stage('Deploy') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl set image deployment/vle7-app vle7-app=$IMAGE'
        sh 'kubectl rollout status deployment/vle7-app --timeout=120s'
      }
    }
  }
}
