pipeline {
    agent any
    stages {
        stage('Print Branch') {
            agent { label 'docker-agent' }
            steps {
                sh '''
                #!/bin/bash
                echo "Branch: ${BRANCH_NAME:-main}"
                '''
            }
        }

        stage('Build on Docker Agent') {
            agent { label 'docker-agent' }
            steps {
                sh '''
                #!/bin/bash
                docker build -t myapp:${BRANCH_NAME:-main} .
                '''
            }
        }

        stage('Build on Instance Agent') {
            agent { label 'ec2-agent' }
            steps {
                sh '''
                #!/bin/bash
                docker build -t myapp:${BRANCH_NAME:-main} .
                '''
            }
        }
    }

    post {
        success {
            sh '''
            #!/bin/bash
            curl -X POST -H 'Content-type: application/json' \
            --data '{"text":"✅ Build succeeded!"}' \
            https://hooks.slack.com/services/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
            '''
        }
        failure {
            sh '''
            #!/bin/bash
            curl -X POST -H 'Content-type: application/json' \
            --data '{"text":"❌ Build failed!"}' \
            https://hooks.slack.com/services/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
            '''
        }
    }
}
