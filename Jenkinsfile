myPlaybook=ping
pipeline {
    agent any

    stages {
        stage('Ansible SyntaxCheck') {
            steps {
                 withCredentials([string(credentialsId: 'BECOME_PASS', variable: 'BECOME_PASS')]) {
                    ansiblePlaybook vaultCredentialsId: 'BECOME_PASS', credentialsId: 'ertanAnsId', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: '$myPlaybook.yml', extras: '-e ansible_become_password=${BECOME_PASS} --syntax-check'
                }
            }
        }
        stage('Test yamllint and ansible-lint') {
            steps {
                sh 'yamllint $myPlaybook.yml'
                sh 'ansible-lint $myPlaybook.yml'
            }
        }
        stage('Execute Ansible') {
            steps {
                 withCredentials([string(credentialsId: 'BECOME_PASS', variable: 'BECOME_PASS')]) {
                    ansiblePlaybook vaultCredentialsId: 'BECOME_PASS', credentialsId: 'ertanAnsId', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: '$myPlaybook.yml', extras: '-e ansible_become_password=${BECOME_PASS}'
                }
            }
        }
        
    }
}
