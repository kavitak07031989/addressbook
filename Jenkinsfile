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

       stage('Copy & Link') {
    when { expression { env.JOB_NAME == 'job-b-deploy' } }
    steps {
        copyArtifacts(
            projectName: 'job-a-build',
            filter: '**/*.war',
            // Remove the 'selector' line entirely
            fingerprintArtifacts: true
        )
        fingerprint '**/*.war'
    }
}
    }
}
