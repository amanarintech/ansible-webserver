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

        // New stage added for running ansible-lint
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

        stage('Third Stage') {
            steps {
                echo "Third stage"  // Placeholder for additional tasks in the third stage
            }
        }
    }
}
