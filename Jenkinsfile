pipeline {
    agent any
 
    stages {
 
       stage('Deploy Juice Shop') {
    steps {
        sh '/usr/local/bin/docker run -d --rm --name juice-shop -p 3000:3000 bkimminich/juice-shop'
    }
}
 
       /* stage('Run ZAP Scan') {
            steps {
                sh '''
                docker run --rm \
                --network host \
                ghcr.io/zaproxy/zaproxy:stable \
                zap-baseline.py \
                -t http://host.docker.internal:3000
                ''' */
        stage('Run ZAP Scan') {
    steps {
        sh '''/usr/local/bin/docker run --rm --network host ghcr.io/zaproxy/zaproxy:stable zap-full-scan.py -t http://host.docker.internal:3000 -r zap-reportr.html -J zap-report.json || true'''
    }
}
            stage('Security Gate') {
    steps {
        script {
            def report = readFile('zap-report.json')
 
            if (report.contains('"riskcode":"3"')) {
                error("Security gate failed: High risk vulnerabilities found")
            } else {
                echo "Security gate passed"
            }
        }
    }
}
 
        
    }
}
