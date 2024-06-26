pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'git@github.com:yop388/https://github.com/yop388/authentication.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                source venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                source venv/bin/activate
                python manage.py test
                '''
            }
        }

        stage('Deploy to Server') {
            steps {
                sshagent(['ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCQf7zG0dbNRERkhDpSICfvCILi0AZwCOfGdbagVw26EZSF0onnEzLFJY0a5mlHElVmmbohAoMo2pf7H2rQqTFoxQI/pJ4Ng3uhxHmvIcHIM8tLEvt+FkKOomOOR60qfbnLv7MGFJl00feFqABDDs2Zog15huv7DUEq0ssZNkGrjE/QUz0LL9vOA+U1aml+SNHRpjRw2dpexEjSDvYTBggL9+RHOpPvo0GBqUY4P/4x3A8rNMv2Leeyht4vMfNqJMT7yXQtPhMgRuyiB9B0ytiEk/TW0EuiVW9KlNztHwHvyLNyuVhkGoYYxXxB1TIY75BAw26fjfg+4afIG6imjqUZZmUoeEOXSKtNerF6aoMseX25baoKQeGOAyXdlTAZfR8icb7luSnriLZNZyRJwqX3fmomycHxwnav6Hc1pBKATksfalay8A0cqPApOzAYq9hiPQzC5CzZ/arGk+bPNsp/KeEk2hgN0i1hSUo4jkaZtbQoQLk3BQ9vDHXZX8RDJNSyTO4dtCS2rJcTYzauURiuMCNC2Y76HmEQReOoOeeXVKyqLhqMwM5t6kR7JKrGu0HaCHvXDgQNmn9nhwdbltWxRfr7MGk7dewwnXsFLi8sSSWFAVwrzuHF5EdsOFfv161qiMcEMtJvjJAPGKc5M505yRsXZSFJ9UXQr+7WrcyzNQ== parfait87y@gmail.com']) {
                    sh '''
                    ssh ubuntu@35.182.247.170 "cd /path/to/your/app && git pull && source /path/to/venv/bin/activate && pip install -r requirements.txt && ./deploy.sh"
                    '''
                }
            }
        }
    }
}

