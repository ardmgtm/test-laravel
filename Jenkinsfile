pipeline {
    agent any
    stages {
        stage('verify version') {
            steps {
                sh 'php --version'
            }
        }
        stage('running') {
            steps {
                sh 'php -S 127.0.0.1:4450 -t "$PWD/public"'
            }
        }
    }
}