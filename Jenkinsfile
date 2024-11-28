pipeline {
    agent { label "agentfarm" }
    stages {
        stage('Delete the workspace') {
            steps {
                cleanWs()
            }
        }
        stages {
        stage('Install Ansible') {
            steps {
                echo 'Updating system and installing Ansible...'
                sh '''
                    sudo apt update && sudo apt upgrade -y
                    sudo apt install -y software-properties-common apt-transport-https wget
                    sudo apt install -y ansible
                    ansible --version
                '''
        stage('Third Stage') {
            steps {
                echo "Third stage"
            }
        }
    }
}

