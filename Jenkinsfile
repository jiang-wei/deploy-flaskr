pipeline {
    agent any
    stages {
         stage('Copy Archive') {
             steps {
                 script {
                     step ([$class: 'CopyArtifact',
                     projectName: 'flaskr',
                     filter: "dist/flaskr-*.tar.gz",
                     target: '.',
                     flatten: true]);
                 }
             }
         }
        stage('deploy staging') {
            steps {
                ansiblePlaybook(
                    installation: 'ansible-2.4.3.0',
                    playbook: 'site.yaml',
                    credentialsId: 'deploy_ssh_private_key',
                    inventory: 'hosts',
                    limit: 'staging',
                    hostKeyChecking: false
                )
            }
        }
        stage('deploy production') {
            input {
                message "continue to production?"
                ok "let's go"
                submitter "jenkins"
            }
            steps {
                ansiblePlaybook(
                    installation: 'ansible-2.4.3.0',
                    playbook: 'site.yaml',
                    credentialsId: 'deploy_ssh_private_key',
                    inventory: 'hosts',
                    limit: 'production',
                    hostKeyChecking: false
                )
            }
        }
    }
}
