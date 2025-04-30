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
                sh """
                    docker run -rm -v /var/lib/docker/volumes/abcd-lab/_data/workspace/ABCD:/zap/wrk zaproxy/zap-stable \
                    bash -c "\
                        zap.sh -cmd -addonupdate; \
                        zap.sh -cmd -addoninstall communityScripts \
                        -addoninstall pscanrulesAlpha \
                        -addoninstall pscanrulesBeta \
                        -autorun /zap/wrk/.zap/passive.yaml" 
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