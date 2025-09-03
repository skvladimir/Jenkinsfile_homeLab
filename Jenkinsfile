pipeline {
    agent any
    environment {
        REMOTE_HOST = '10.10.10.70'
    }
    stages {
        stage('Install Apache2') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'debian-pass', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    sshpass -p $PASS ssh -o StrictHostKeyChecking=no $USER@$REMOTE_HOST 'apt update && apt install apache2 -y && systemctl enable apache2 && systemctl start apache2'
                    """
                }
            }
        }

        stage('Check OS') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'debian-pass', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "sshpass -p $PASS ssh -o StrictHostKeyChecking=no $USER@$REMOTE_HOST 'uname -a'"
                }
            }
        }

        stage('Check Logs') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'debian-pass', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    sshpass -p $PASS ssh -o StrictHostKeyChecking=no $USER@$REMOTE_HOST 'grep -E " 4[0-9]{2}| 5[0-9]{2}" /var/log/apache2/access.log || echo No 4xx/5xx errors found'
                    """
                }
            }
        }
    }
}
