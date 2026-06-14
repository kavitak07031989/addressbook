pipeline {
    agent any
    stages {
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('package') {
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Fingerprint') { // This must be INSIDE 'stages'
            steps {
                fingerprint 'target/*.jar'
            }
        }
       stage('Copy and Link') {
    steps {
        copyArtifacts(
            projectName: 'job-a-build', 
            filter: '**/*.jar', // Use the wildcard to ensure it matches
            fingerprintArtifacts: true 
        )
    }
}
    }
}
