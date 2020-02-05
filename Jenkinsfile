
pipeline {
  agent {
    kubernetes {
      cloud 'openshift'
      label 'nodejs'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: node
    image: node:12.9.0-alpine
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