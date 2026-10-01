pipeline {

  environment {
    PROJECT = "my-demo-507516"
    APP_NAME = "gceme"
    FE_SVC_NAME = "${APP_NAME}-frontend"
    CLUSTER = "jenkins-cd"
    CLUSTER_ZONE = "us-east1-a"
    IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    JENKINS_CRED = "${PROJECT}"
  }

  agent {
    kubernetes {
      label 'sample-app'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    component: ci
spec:
  serviceAccountName: cd-jenkins
  containers:
  - name: golang
    image: golang:1.10
    command: ['cat']
    tty: true
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command: ['cat']
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command: ['cat']
    tty: true
"""
    }
  }

  stages {

    stage('Test') {
      steps {
        container('golang') {
          sh """
            ln -s `pwd` /go/src/sample-app
            cd /go/src/sample-app
            go test
          """
        }
      }
    }

    stage('Build (Demo Only)') {
      steps {
        echo "Skipping image build for demo"
        sh 'sleep 2'
        echo "Pretend build complete"
      }
    }

    stage('Deploy (Skipped)') {
      steps {
        echo "Skipping deploy for demo"
      }
    }

  }
}
