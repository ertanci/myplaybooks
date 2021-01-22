pipeline {
    agent any

    stages {
        stage('SCM Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ertanci/myplaybooks'
            }
        }
        stage('Ansible SyntaxCheck') {
            steps {
                 withCredentials([string(credentialsId: 'BECOME_PASS', variable: 'BECOME_PASS')]) {
                    ansiblePlaybook vaultCredentialsId: 'BECOME_PASS', credentialsId: 'ertanAnsId', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: '${YML_NAME}.yml', extras: '-e ansible_become_password=${BECOME_PASS} --syntax-check'
                }
            }
        }
        stage('Test yamllint and ansible-lint') {
            steps {
                sh 'yamllint ${YML_NAME}.yml'
                sh 'ansible-lint ${YML_NAME}.yml'
            }
        }
        stage('Execute Ansible') {
            steps {
                 withCredentials([string(credentialsId: 'BECOME_PASS', variable: 'BECOME_PASS')]) {
                    ansiblePlaybook vaultCredentialsId: 'BECOME_PASS', credentialsId: 'ertanAnsId', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: '${YML_NAME}.yml', extras: '-e ansible_become_password=${BECOME_PASS}'
                }
            }
        }
        
    }
}
