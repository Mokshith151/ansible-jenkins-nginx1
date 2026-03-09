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
                sh 'ansible-playbook playbooks/install_nginx.yml --syntax-check'
            }
        }

        stage('Run Playbook') {
            steps {
                sh '''
                    ansible-playbook playbooks/install_nginx.yml \
                    -i inventory/hosts \
                    -u ${ANSIBLE_USER} \
                    --private-key=${PRIVATE_KEY}
                '''
            }
        }

        stage('Verify Nginx') {
            steps {
                sh 'ansible managed_nodes -i inventory/hosts -m command -a "systemctl is-active nginx"'
            }
        }

    }

    post {
        success {
            echo 'Nginx installed successfully!'
        }
        failure {
            echo 'Pipeline failed — check logs!'
        }
    }
}