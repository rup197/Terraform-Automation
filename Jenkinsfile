pipeline {
    agent any

    parameters {
        choice(
            name: 'ACTION',
            choices: ['plan', 'apply'],
            description: 'Select the action to perform'
        )
        string(
            name: 'BRANCH_NAME',
            defaultValue: 'feature-branch',
            description: 'Name of the branch to create'
        )
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ygminds73/Terraform-Automation.git']])
            }
        }
    
        stage('Create Branch') {
            steps {
                sh "git checkout -b ${params.BRANCH_NAME}"
            }
        }

        stage ("terraform init") {
            steps {
                sh ("terraform init -reconfigure") 
            }
        }

        stage ("Action") {
            steps {
                script {
                    switch (params.ACTION) {
                        case 'plan':
                            echo 'Executing Plan...'
                            sh "terraform plan"
                            break
                        case 'apply':
                            echo 'Executing Apply...'
                            sh "terraform apply --auto-approve"
                            break
                        default:
                            error 'Unknown action'
                    }
                }
            }
        }
    }
}
