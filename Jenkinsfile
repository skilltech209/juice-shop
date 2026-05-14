pipeline {
    agent any
 
    stages {
 
       stage('Deploy Juice Shop') {
    steps {
        sh '/usr/local/bin/docker run -d --rm --name juice-shop -p 3000:3000 bkimminich/juice-shop'
    }
}
 
        stage('Run ZAP Scan') {
            steps {
                sh '''
                docker run --rm \
                --network host \
                ghcr.io/zaproxy/zaproxy:stable \
                zap-baseline.py \
                -t http://host.docker.internal:3000
                '''
            }
        }
    }
}
