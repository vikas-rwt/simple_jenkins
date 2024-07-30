pipeline {
    agent any
    environment {
        DB_URL = 'mysql+pymysql://usr:pwd@host:<port>/db'
        DISABLE_AUTH = true
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-access-secret-key')
    }
    stages {
        stage("Build") {
            steps {
                echo "Building the app..."
                sh '''
                    echo "This block contains multi-line steps"
                    ls -lh
                '''
                sh '''
                    echo "Database url is: ${DB_URL}"
                    echo "DISABLE_AUTH is ${DISABLE_AUTH}"
                    env
                '''
                echo "Running a job with build #: ${env.BUILD_NUMBER} on ${env.JENKINS_URL}"
            }
        }
        stage("Test") {
            steps {
                echo "Testing the app..."
            }
        }
        stage("Deploy to Staging") {
            steps {
                sh 'chmod u+x deploy smoke-tests'
                sh './deploy staging'
                sh './smoke-tests'
            }
        }
        stage("Sanity Check") {
            steps {
                input "Should we ship to prod?"
            }
        }
        stage("Deploy to Production") {
            steps {
                sh './deploy prod'
            }
        }
    }
}
