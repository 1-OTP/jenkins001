pipeline {
    agent any

    environment {
        NMAP_IMAGE = 'instrumentisto/nmap'
        TARGET = 'youtube.com'  // Replace with your target
    }

    stages {
stage('Prepare') {
    steps {
        script {
            if (isUnix()) {
                sh 'docker --version'
            } else {
                bat 'docker --version'
            }
        }
    }
}
    }

}