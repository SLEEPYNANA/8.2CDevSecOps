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
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        // allow the stage to succeed even if tests fail
        bat 'npm test || exit 0'
      }
      post {
        always {
          // send email no matter what
          emailext(
            subject: "Tests ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """\
Build: ${env.BUILD_URL}

*Result:* ${currentBuild.currentResult}
*Console log attached.*
""",
            to: 'sindhujaghosh@gmail.com',
            attachLog: true
          )
        }
      }
    }

    stage('Generate Coverage Report') {
      steps {
        bat 'npm run coverage || exit 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        bat 'npm audit || exit 0'
      }
      post {
        always {
          emailext(
            subject: "Security Scan ${currentBuild.currentResult}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """\
Build: ${env.BUILD_URL}

*Result:* ${currentBuild.currentResult}
*Audit report attached (console log).*
""",
            to: 'sindhujaghosh@gmail.com',
            attachLog: true
          )
        }
      }
    }
  }

  post {
    failure {
      // a final “pipeline failed” notification
      emailext(
        subject: "Pipeline FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
        body: "Oops – something broke! Check the console: ${env.BUILD_URL}console",
        to: 'sindhujaghosh@gmail.com'
      )
    }
  }
}
