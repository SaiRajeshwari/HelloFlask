def workspace = pwd()
pipeline {
  agent { docker { image 'python:3.7.2' } }
  stages {
    stage('Build') {
      steps {
        sh 'pip install -r requirements.txt'
        sh "echo 'Running ${env.BUILD_ID} on ${env.JENKINS_URL}'"
        sh "echo 'Workspace ${workspace}'"
      }
    }
    stage('Code Analysis') {
      steps {
        sh "echo 'Run Static Code Analysis'"
        build job: 'Code Analysis', parameters: [string(name: 'workspace', value: workspace)]
      }
    }
    stage('Test') {
      steps {
        sh 'python test.py'
      }
      post {
        always {
          junit 'test-reports/*.xml'
        }
      }    
    }
    stage('Deploy') {
      steps {
        sh "echo 'Ready to deploy!'"
        build 'Deploy'
      }
    }
  }
}