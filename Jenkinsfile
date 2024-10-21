pipeline {
    agent any

    environment {
        IMAGE_NAME = 'my-app-image'       // Name of your Docker image
        TRIVY_REPORT = 'trivy-report.json' // Output report for Trivy
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'git@github.com:Daimi5565/DevSecOps.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Scan Image with Trivy') {
            steps {
                script {
                    // Scan the image with Trivy
                    sh "trivy image --format json -o ${TRIVY_REPORT} ${IMAGE_NAME}:latest"
                }
            }
        }

        stage('Archive Trivy Report') {
            steps {
                // Archive the report so you can access it later
                archiveArtifacts artifacts: 'trivy-report.json', allowEmptyArchive: true
            }
        }

        stage('Publish Results') {
            steps {
                // Print report to the console (optional)
                sh "cat trivy-report.json"
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean up the workspace after the job
        }
    }
}

