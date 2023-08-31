node {
    def dockerImage = 'node:16-buster-slim'

    stage('Checkout') {
        checkout scm
    }

    stage('Build') {
        docker.image(dockerImage).inside("-p 3010:3010") {
            sh 'npm install'
        }
    }

    stage('Test') {
        docker.image(dockerImage).inside("-p 3010:3010") {
            sh './jenkins/scripts/test.sh'
        }
    }

    stage('Manual Approval') {
        input message: 'Apakah Anda ingin melanjutkan ke tahap Deploy? (Klik "Proceed" untuk melanjutkan)'
    }

    stage('Deploy') {
        docker.image(dockerImage).inside("-p 3010:3010") {
            sh './jenkins/scripts/deliver.sh'
            input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
            sh 'sleep 1m'
            sh './jenkins/scripts/kill.sh'
        }
    }

    // Add more stages as needed
}