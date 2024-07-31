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
                sh "docker run --rm ${NMAP_IMAGE} -sV -p- ${TARGET_URL} -oX security_reports/nmap_results.xml"
            }
        }

        stage('Vulnerability Scanning') {
            steps {
                // Run OWASP ZAP scan
                sh """
                docker run --rm -v \$(pwd)/security_reports:/zap/wrk:rw ${ZAP_IMAGE} zap-baseline.py \
                    -t ${TARGET_URL} -g gen.conf -r zap_report.html
                """
            }
        }

        stage('Analyze Results') {
            steps {
                // Example: Count high-risk findings in Nmap results
                sh 'grep "high" security_reports/nmap_results.xml | wc -l'
                
                // Example: Check for high-risk findings in ZAP report
                sh 'grep "High" security_reports/zap_report.html | wc -l'
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