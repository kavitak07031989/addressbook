pipeline {
    agent any
    stages {
        // Build stage for job-a-build
        stage('Build & Archive') {
            when { expression { env.JOB_NAME == 'job-a-build' } }
            steps {
                sh 'mvn clean package'
                // Archive the .war file produced by Maven
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        // Copy stage for job-b-deploy
        stage('Copy & Link') {
            when { expression { env.JOB_NAME == 'job-b-deploy' } }
            steps {
                copyArtifacts(
                    projectName: 'job-a-build',
                    filter: 'target/*.war', // Target the .war file
                    fingerprintArtifacts: true
                )
                // Explicitly verify the fingerprint to create the link
                fingerprint 'target/*.war'
            }
        }
    }
}
