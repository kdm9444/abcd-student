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
        stage('Run JuiceShop'){
            steps{
                sh 'docker run -d --rm --name juice-shop bkimminich/juice-shop' 
                sh 'docker ps'
            }
        }
        stage('Run ZAP DAST Scan'){
            steps{
                sh 'export REPORT_TITLE="report_$(date +%s)"'
                sh 'echo $REPORT_TITLE'
                sh 'ls -la $(pwd)'
                sh """
                    docker run --rm --name zaproxy -v ${env.WORKSPACE}:/zap/wrk zaproxy/zap-stable ls -la /zap/wrk
                    """
            }
        }
        stage('Check docker'){
            steps{
                sh 'docker ps'
            }
        }
    }
    post {
        always {
            echo "Cleaning up..."
            sh "docker container stop juice-shop || true"
        }
    }
}