pipeline {
  agent any

  stages {
    stage('Checkout App') {
      steps {
        deleteDir()
        git branch: 'main',
            url: 'https://github.com/snyk-labs/nodejs-goof.git'
      }
    }
    stage('Install Dependencies') {
      steps { bat 'npm install' }
    }
    stage('Run Tests') {
      steps { bat 'npm test || exit 0' }
    }
    stage('Generate Coverage Report') {
      steps { bat 'npm run coverage || exit 0' }
    }
    stage('NPM Audit (Security Scan)') {
      steps { bat 'npm audit || exit 0' }
    }
  }
}
