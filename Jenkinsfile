pipeline {
    agent any
    stages {
        stage('Build & Archive') {
            when { expression { env.JOB_NAME == 'job-a-build' } }
            steps {
                sh 'mvn clean package'
                archiveArtifacts artifacts: 'target/addressbook.war', fingerprint: true
            }
        }
        stage('Copy & Link') {
            when { expression { env.JOB_NAME == 'job-b-deploy' } }
            steps {
                copyArtifacts(
                    projectName: 'job-a-build',
                    filter: 'target/addressbook.war',
                    fingerprintArtifacts: true
                )
                fingerprint 'target/addressbook.war'
            }
        }
        stage('Docker Build') {
            when { expression { env.JOB_NAME == 'job-b-deploy' } }
            steps {
                script {
                    // This builds the image using the Dockerfile in your root folder
                    docker.build("addressbook-app:${env.BUILD_NUMBER}")
                }
            }
        }
    }
}
