// Scripted pipeline
properties([
  pipelineTriggers([
    pollSCM('H/2 * * * *') // cek repo setiap 2 menit
  ])
])

node {
  stage('Checkout') {
    // Ambil source code dari repo & branch yg di-set di Jenkins job
    checkout scm
  }

  stage('Build') {
    docker.image('node:lts-buster-slim').inside('-p 3000:3000') {
      withEnv(['CI=true']) {
        sh 'npm install'
      }
    }
  }

  stage('Test') {
    docker.image('node:lts-buster-slim').inside('-p 3000:3000') {
      sh './jenkins/scripts/test.sh'
    }
  }

  stage('Deliver') {
    docker.image('node:lts-buster-slim').inside('-p 3000:3000') {
      sh './jenkins/scripts/deliver.sh'
      input message: 'Finished using the website? (Click "Proceed" to continue)'
      sh './jenkins/scripts/kill.sh'
    }
  }
}
