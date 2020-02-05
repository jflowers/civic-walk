
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
  - name: node
    image: nodejs:12
    command:
    - cat
    tty: true
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