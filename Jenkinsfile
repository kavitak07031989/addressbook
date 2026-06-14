pipeline {
    agent any
    stages {
        // This stage only runs if the job is named 'job-a-build'
        stage('Build & Archive') {
            when { expression { env.JOB_NAME == 'job-a-build' } }
            steps {
                sh 'mvn clean package'
                archiveArtifacts artifacts: '**/*.jar', fingerprint: true
            }
        }

        // This stage only runs if the job is named 'job-b-deploy'
        stage('Copy & Link') {
            when { expression { env.JOB_NAME == 'job-b-deploy' } }
            steps {
                copyArtifacts(
                    projectName: 'job-a-build',
                    filter: '**/*.jar',
                    fingerprintArtifacts: true // This creates the fingerprint record!
                )
            }
        }
    }
}
