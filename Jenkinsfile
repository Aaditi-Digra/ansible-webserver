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
                    credentialsId: 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQC6CJCH+ncEPfLWoKeJqOjFIm0+nhfn2tGXaFPoitDPB1VQMxcJTDFak2AFNvnM8oUiWD9WT3XzBZNEK09/7/lW1QfEr3J9iYNU2DPP08YpPK6rN/yWDGn5x0sdlCYtQ7PFsWqr0nU+u/RKBiDiEs+gZGZQDri2mniP73XcKRplldrk3frF51IvU7zMz6LHQ+FAG12WoGiHbLctzMwfMzaPzbcpjNNiX92jcpgIRZpoOYf19wWdVbVkLflP4YAfGnKqXphnggW+Y1AE32ESSNAyQVOBHCY/vWxoXOrJLqxZlji6wyie8+peAi57FQm9VxTgvdP2UIIWn4V0Yk+fLXyA6WWtO/NFcdKp5Q3iiKZ+/NLjlsNSA9BCcdVCTwC/6P5LyCP9jVV/g3Bq2CNPLsPYcfDo3XWeEcNixX7sk5MrTHjwr8r8DZ1nvgnr/s40N8WiKc4Npb9oyrotm8aBI/T6SFqHzT3ktbt6QbLxyboA4Jveni3l1jHnpLa7q05l5j13MMYyihK6ibFOd0ZqzZdUncIKkC2G2aaY40OPIo4mPP7YAC8dG19nASoWuAmCcSwfJjJbZUq7scXFtWTzSZ681J9mz+gZSyMVY2AKo8ASCQcnv/DoCVQW4feVPtc3bh5XyVK0nCsZPSg/S2TV3eC5hpiNoL2l6qh9BotzJxAXtw== jenkins-agent',
                    url: 'git@github.com:Aaditi-Digra/ansible-webserver.git'
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
