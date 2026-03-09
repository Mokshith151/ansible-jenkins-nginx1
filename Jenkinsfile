pipeline {

    agent any

    environment {
        JENKINS_TRIGGERED = 'true'
        ANSIBLE_USER      = 'ansible'
        PRIVATE_KEY       = credentials('ansible-ssh-key')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Syntax Check') {
            steps {
                sh 'ansible-playbook playbooks/install_nginx.yml --syntax-check -i /etc/ansible/hosts'
            }
        }

        stage('Run Playbook') {
            steps {
                sh '''
                    export JENKINS_TRIGGERED=true
                    ansible-playbook playbooks/install_nginx.yml \
                    -i /etc/ansible/hosts \
                    -u ${ANSIBLE_USER} \
                    --private-key=${PRIVATE_KEY}
                '''
            }
        }

        stage('Verify Nginx') {
            steps {
                sh 'ansible dev -i /etc/ansible/hosts -m command -a "systemctl is-active nginx"'
            }
        }

    }

    post {
        success {
            echo 'Nginx installed successfully!'
        }
        failure {
            echo 'Pipeline failed - check logs!'
        }
    }
}
