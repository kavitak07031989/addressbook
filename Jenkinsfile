pipeline {
    agent any
    stages {
        stage('Build & Archive')A {
            when { expression { env.JOB_NAME == 'job-a-build' } }
            steps {
                sh 'mvn clean package'
                // Use a direct path so Jenkins doesn't lose the file
                archiveArtifacts artifacts: 'target/addressbook.war', fingerprint: true
            }
        }
        stage('Copy & Link') {
            when { expression { env.JOB_NAME == 'job-b-deploy' } }
            steps {
                // Ensure we copy from the specific target folder
                copyArtifacts(
                    projectName: 'job-a-build',
                    filter: 'target/addressbook.war',
                    fingerprintArtifacts: true
                )
                // Register the file specifically
                fingerprint 'target/addressbook.war'
            }
        }
    }
}
