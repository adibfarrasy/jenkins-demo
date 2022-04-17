pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE = 'sqlite'
    }
    stages {
        stage('Build') {
            steps {
                echo "Database engine is $DB_ENGINE"
                echo "DISABLE_AUTH is $DISABLE_AUTH"
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy - Staging') {
            when {
                branch 'staging'
            }
            steps {
                sh './deploy staging'
                sh './run-smoke-tests'
            }
        }
        stage('Deploy - Production') {
            when {
                branch 'main'
            }
            steps {
                sh './deploy production'
            }
        }
    }
}