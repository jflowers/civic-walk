
pipeline {
  agent {
    kubernetes {
      cloud 'openshift'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: jnlp
    image: openshift/ose-jenkins-agent-base:v4.2.15
    args: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
"""
    }
  }
  options {
    timeout(time: 20, unit: 'MINUTES')
  }

  stages {
    stage('preamble') {
      steps {
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