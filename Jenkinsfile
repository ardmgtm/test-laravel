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
                sh 'php -S 12.7.108.141:4450 -t "$PWD/public"'
            }
        }
    }
}