pipeline {

  environment {
    PROJECT = "useful-cathode-334010"
    APP_NAME = "hello"
    FE_SVC_NAME = "${APP_NAME}-frontend"
    CLUSTER = "cluster1"
    CLUSTER_ZONE = "us-central-c"
    IMAGE_TAG = "gcr.io/${PROJECT}/${APP_NAME}:${env.BRANCH_NAME}.${env.BUILD_NUMBER}"
    JENKINS_CRED = "${PROJECT}"
  }

  agent {
    kubernetes {
      inheritFrom'sample-app'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  # serviceAccountName: cd-jenkins
  containers:
  - name: java
    image: openjdk:latest
    command:
    - cat
    tty: true
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
}
  }
  stages {
    stage('build') {
      steps {
        container('java') {
          sh """
            ln -s `pwd` /go/src/sample-app
            cd /go/src/sample-app
            go test
          """
        }
      }
    }
    stage('Build and push image with Container Builder') {
      steps {
        container('gcloud') {
          sh "PYTHONUNBUFFERED=1 gcloud builds submit -t ${IMAGE_TAG} ."
        }
      }
    }
   
    stage('Deploy Production') {
      steps{
        container('kubectl') {

        }
      }
    }
    stage('Deploy Dev') {

      }
      steps {
        container('kubectl') {

        }
      }
    }
  }
}
