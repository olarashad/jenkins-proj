pipeline {
    agent any
    stages {
        stage('Print Branch') {
            agent { label 'docker-agent' }
            steps {
                echo "Branch: ${env.BRANCH_NAME ?: 'main'}"
            }
        }

        stage('Build on Docker Agent') {
            agent { label 'docker-agent' }
            steps {
                sh 'docker build -t myapp:${env.BRANCH_NAME ?: "main"} .'
            }
        }

        stage('Build on Instance Agent') {
            agent { label 'ec2-agent' }
            steps {
                sh 'docker build -t myapp:${env.BRANCH_NAME ?: "main"} .'
            }
        }
    }

    post {
        success {
            sh """
              curl -X POST -H 'Content-type: application/json' \
              --data '{"text":"✅ Build succeeded!"}' \
              https://hooks.slack.com/services/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
            """
        }
        failure {
            sh """
              curl -X POST -H 'Content-type: application/json' \
              --data '{"text":"❌ Build failed!"}' \
              https://hooks.slack.com/services/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
            """
        }
    }
}
