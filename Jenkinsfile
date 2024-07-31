pipeline {
    agent any

    environment {
        NMAP_IMAGE = 'instrumentisto/nmap'
        TARGET = 'youtube.com'  // Replace with your target
    }

    stages {
        stage('Prepare') {
            steps {
                // Check Docker version
                bat 'docker --version'
                
                // Pull Nmap Docker image
                bat "docker pull ${NMAP_IMAGE}"
                
                // Create directory for scan results
                bat 'if not exist nmap_results mkdir nmap_results'
            }
        }

        stage('Run Nmap Scan') {
            steps {
                // Run Nmap scan using Docker
                bat """
                docker run --rm ^
                    -v %CD%\\nmap_results:/output ^
                    ${NMAP_IMAGE} ^
                    -sV -p- -oN /output/nmap_scan.txt ${TARGET}
                """
            }
        }

        stage('Analyze Results') {
            steps {
                // Example: Count open ports (using Windows commands)
                bat 'findstr "open" nmap_results\\nmap_scan.txt | find /c /v ""'
                
                // Example: Check for specific service
                bat 'findstr "http" nmap_results\\nmap_scan.txt || echo No HTTP service found'
            }
        }
    }

    post {
        always {
            // Archive the Nmap results
            archiveArtifacts artifacts: 'nmap_results/**/*', fingerprint: true
        }
    }
}