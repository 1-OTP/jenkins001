pipeline {
    agent any

    environment {
        NMAP_IMAGE = 'instrumentisto/nmap'
        TARGET = 'youtube.com'  // Replace with your target
    }

    stages {
        stage('Prepare') {
            steps {
                // Ensure Docker is available
                sh 'docker --version'
                
                // Pull Nmap Docker image
                sh "docker pull ${NMAP_IMAGE}"
                
                // Create directory for scan results
                sh 'mkdir -p nmap_results'
            }
        }

    }

}