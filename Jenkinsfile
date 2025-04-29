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
            agent {
                docker {
                    image 'zaproxy/zap-stable'
                    args '-v $(pwd):/zap/wrk/'
                }
            }
            steps{
                sh 'export REPORT_TITLE="report_$(date +%s)"'
                sh 'echo $REPORT_TITLE'
                sh 'ls -la'
                sh 'ls -la /zap/wrk/'
                sh 'bash -c "\
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