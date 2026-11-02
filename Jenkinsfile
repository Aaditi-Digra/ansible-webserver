pipeline {
    agent { label 'linux' }

    options {
        skipDefaultCheckout()
    }

    stages {
        stage('Delete the workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout code') {
            steps {
                git branch: 'main',
                    credentialsId: '',
                    url: 'https://github.com/Aaditi-Digra/ansible-webserver.git'
            }
        }

        stage('Installing Ansible') {
            steps {
                sh '''
                    if command -v ansible >/dev/null && command -v ansible-lint >/dev/null; then
                        echo "Skipping install - ansible and ansible-lint already exist"
                    else
                        sudo apt-get update -y
                        sudo apt-get install -y wget tree unzip ansible ansible-lint python3-pip python3-apt
                    fi
                '''
            }
        }

        stage('Run ansible-lint against playbook') {
            steps {
                sh '''
                    cd playbooks
                    ansible-lint apache-install.yml website-update.yml website-test.yml
                '''
            }
        }
    }
}
