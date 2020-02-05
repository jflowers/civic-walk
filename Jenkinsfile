
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
    image: openshift4/ose-jenkins-agent-base:v4.2.15
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
  - name: node
    image: rhel8/nodejs-12:1-27
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
    stage('preamble') {
      steps {
        container('node'){
          sh 'npm -h'
        }
        script {
          openshift.withCluster() {
            openshift.withProject() {
              echo "Using project: ${openshift.project()}"
              echo "..."
            }
          }
        }
      }
    }
  }
}