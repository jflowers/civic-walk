
pipeline {
  agent {
    kubernetes {
      cloud 'openshift'
      label 'civic-walk-pipeline-agent'
      yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    worker: civic-walk-pipeline-agent
spec:
  containers:
  - name: jnlp
    image: registry.redhat.io/openshift4/ose-jenkins-agent-base:v4.2.15
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
  - name: node
    image: registry.redhat.io/rhel8/nodejs-12:1-27
    command:
    - cat
    tty: true
  imagePullSecrets:
    - name: 12968862-jenkins-pull-secret
"""
    }
  }
  options {
    timeout(time: 20, unit: 'MINUTES')
  }

  stages {
    stage('Build') {
      steps {
        container('node'){
          dir('frontend'){
            sh 'npm ci'
            sh 'npm run build'
          }
        }
      }
    }
    stage('Unit Test') {
      steps {
        container('node'){
          dir('frontend'){
            sh 'npm run test'
          }
        }
      }
    }
  }
}