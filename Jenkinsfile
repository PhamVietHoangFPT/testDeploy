pipeline {
    environment {
  VERCEL_TOKEN = 'ooKbttGktBsG83UCDBqoHGBr'
}
  agent any
  tools {nodejs "NodeJS Test"}
  stages {
    stage('Build') {
      steps {
        git branch: 'TestV2', url: 'https://github.com/PhamVietHoangFPT/testDeploy.git'

        bat 'npm install'
      }
    }
    stage('Deploy to Vercel') {
      steps {
        // Install the Vercel CLI globally
        bat 'npm install -g vercel'

        bat 'vercel logout'
        bat 'vercel login'
        bat 'vercel --prod --yes --token=%VERCEL_TOKEN% --name=phamviethoangfpts-projects --force'
      }
  }
      stages {
        stage('Example') {
            steps {
                gerritReview labels: [Verified: 0]
                echo 'Hello World'
                gerritComment path:'https://github.com/PhamVietHoangFPT/testDeploy/edit/main/Jenkinsfile', line: 30, message: 'Hello'
            }
        }
    }
    post {
        success {
            gerritReview labels: [Verified: 1]
            gerritCheck checks: ['example:checker': 'SUCCESSFUL']
        }
        unstable { gerritReview labels: [Verified: 0], message: 'Build is unstable' }
        failure {
            gerritReview labels: [Verified: -1]
            gerritCheck checks: ['example:checker': 'FAILED'], message: 'invalid syntax'
        }
    }
  }
}
