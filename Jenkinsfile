pipeline {
    agent any

    environment {
        NMAP_IMAGE = 'instrumentisto/nmap'
        ZAP_IMAGE = 'owasp/zap2docker-stable'
        TARGET_URL = 'youtube.com'  // Replace with your target
    }

    stages {
        stage('Prepare Environment') {
            steps {
                // Ensure Docker is available
                sh 'docker --version'
                
                // Pull necessary Docker images
                sh "docker pull ${NMAP_IMAGE}"
                sh "docker pull ${ZAP_IMAGE}"
                
                // Create a directory for reports
                sh 'mkdir -p security_reports'
            }
        }

        stage('Network Scanning') {
            steps {
                // Run Nmap scan
                sh "docker run --rm ${NMAP_IMAGE} -sV -p- ${TARGET_URL}"
            }
        }
    }

    post {
        always {
            // Archive the reports
            archiveArtifacts artifacts: 'security_reports/**/*', fingerprint: true
        }
    }
}