
pipeline {
  agent {
    openshift {
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: jnlp
    image: ose-jenkins-agent-base:v4.2.15
    command: ['\$(JENKINS_SECRET)', '\$(JENKINS_NAME)']
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