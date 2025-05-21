pipeline {
  agent any

  stages {
    stage('Checkout App') {
      steps {
        deleteDir()
        git url: 'https://github.com/SLEEPYNANA/8.2CDevSecOps', branch: 'main'
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
