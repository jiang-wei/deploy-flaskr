pipeline {
    agent any
    stages {
         stage('Copy Archive') {
             steps {
                 script {
                     step ([$class: 'CopyArtifact',
                     projectName: 'flaskr',
                     filter: "dist/flaskr-*.tar.gz",
                     target: '.']);
                 }
             }
         }
        stage('deploy staging') {
            steps {
                sh '/home/vagrant/env_ansible/bin/ansible-playbook -i hosts -l staging site.yaml'
            }
        }
        stage('deploy production') {
            input {
                message "continue to production?"
                ok "let's go"
                submitter "jenkins"
            }
            steps {
                sh '/home/vagrant/env_ansible/bin/ansible-playbook -i hosts -l production site.yaml'
            }
        }
    }
}
