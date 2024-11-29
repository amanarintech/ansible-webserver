pipeline {
    agent { label "agentfarm" }
    stages {
        stage('Delete the workspace') {
            steps {
                cleanWs()  // Clean up the workspace before proceeding
            }
        }
        
        stage('Installing Ansible') {
            steps {
                script {
                    def ansible_exists = fileExists('/usr/bin/ansible')  // Check if Ansible is already installed
                    if (ansible_exists == true) {
                        echo "Skipping Ansible install - already exists"
                    } else {
                        // Install necessary packages including Ansible if not already installed
                        sh 'sudo apt-get update -y && sudo apt-get upgrade -y'
                        sh 'sudo apt install -y wget tree unzip ansible python3-pip python3-apt'
                    }
                }
            }
        }

        // New stage added for downloading Ansible code
        stage('Download Ansible Code') {
            steps {
                script {
                    // Command to download the Ansible code
                    echo "Downloading Ansible code.."
                    git credentialsId: 'git-repo-creds', url: 'git@github.com:amanarintech/ansible-webserver.git'
                }
            }
        }


        // Move the linting stage to the last position
        stage('Run ansible-lint against playbooks') {
            steps {
                script {
                    // Run ansible-lint using docker for each playbook
                    sh 'docker run --rm -v $WORKSPACE/playbooks:/data cytopia/ansible-lint:4 apache-install.yml'
                    sh 'docker run --rm -v $WORKSPACE/playbooks:/data cytopia/ansible-lint:4 website-update.yml'
                    sh 'docker run --rm -v $WORKSPACE/playbooks:/data cytopia/ansible-lint:4 website-test.yml'
                }
            }
        }
    }
}
