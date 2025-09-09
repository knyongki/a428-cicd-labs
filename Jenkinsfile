properties([
  pipelineTriggers([
    pollSCM('H/2 * * * *') // cek repo setiap 2 menit
  ])
])

node {
  stage('Build') {
    withEnv(['CI=true']) {
      sh 'npm install'
    }
  }

  stage('Test') {
    sh './jenkins/scripts/test.sh'
  }

  stage('Deliver') {
    sh './jenkins/scripts/deliver.sh'
    input message: 'Finished using the website? (Click "Proceed" to continue)'
    sh './jenkins/scripts/kill.sh'
  }
}
