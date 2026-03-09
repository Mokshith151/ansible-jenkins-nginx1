pipeline {

    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
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
                sh 'ansible-playbook playbooks/install_nginx.yml -i /etc/ansible/hosts'
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
