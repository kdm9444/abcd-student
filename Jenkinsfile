pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: 'github-pat', url: 'https://github.com/kdm9444/abcd-student', branch: 'main'
                }
            }
        }
        stage('Example') {
            steps {
                echo 'Hello!'
                sh 'ls -la'
            }
        }
        stage('Run ZAP DAST Scan'){
            steps{
                sh 'export REPORT_TITLE="report_$(date +%s)"'
                sh 'echo $REPORT_TITLE'
                sh 'docker run --rm -e "REPORT_TITLE=$REPORT_TITLE" -v "$(pwd):/zap/wrk/" --name owasp zaproxy/zap-stable \
                    bash -c "\
                    ls -la ./wrk"'
            }
        }
    }
}