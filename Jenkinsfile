/* Requires the Docker Pipeline plugin */
pipeline {
    agent { docker { image 'php:8.4.1-alpine3.20' } }
    stages {
        stage('build') {
            steps {
                sh 'php --version'
            }
        }
    }
}