pipeline {
    agent any

    stages {
        stage('Ansible SyntaxCheck') {
            steps {
                 withCredentials([string(credentialsId: 'BECOME_PASS', variable: 'BECOME_PASS')]) {
                    ansiblePlaybook vaultCredentialsId: 'BECOME_PASS', credentialsId: 'ertanAnsId', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'ping.yml', extras: '-e ansible_become_password=${BECOME_PASS} --syntax-check'
                }
            }
        }
        stage('Test yamllint and ansible-lint') {
            steps {
                sh 'yamllint ping.yml'
                sh 'ansible-lint ping.yml'
            }
        }
        stage('Execute Ansible') {
            steps {
                 withCredentials([string(credentialsId: 'BECOME_PASS', variable: 'BECOME_PASS')]) {
                    ansiblePlaybook vaultCredentialsId: 'BECOME_PASS', credentialsId: 'ertanAnsId', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'ping.yml', extras: '-e ansible_become_password=${BECOME_PASS}'
                }
            }
        }
        
    }
}
