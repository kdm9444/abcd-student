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
                sh 'ls -la $(pwd)/.zap'
                sh 'docker run --rm -e "AUTH_TOKEN=$AUTH_TOKEN" -e "REPORT_TITLE=$REPORT_TITLE" -v "$(pwd):/zap/wrk/" zaproxy/zap-stable \
                    bash -c "\
                    ls -la
                    ls -la /zap/wrk
                    zap.sh -cmd -addonupdate; \
                    zap.sh -cmd -addoninstall communityScripts \
                    -addoninstall pscanrulesAlpha \
                    -addoninstall pscanrulesBeta \
                    -addoninstall graaljs \
                    -autorun /zap/wrk/.zap/passive.yaml"'
            }
        }
    }
}