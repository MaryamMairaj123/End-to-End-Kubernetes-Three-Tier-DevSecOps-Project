properties([
    parameters([
        string(
            defaultValue: 'dev',
            name: 'Environment'
        ),
        choice(
            choices: ['plan', 'apply', 'destroy'], 
            name: 'Terraform_Action'
        )])
])
pipeline {
    agent any
    stages {
        stage('Preparing') {
            steps {
                sh 'echo Preparing'
            }
        }
        stage('Git Pulling') {
            steps {
                git branch: 'main', url: 'https://github.com/MaryamMairaj123/EKS-Terraform-GitHub-Actions-master.git'
            }
        }
        stage('Init') {
            steps {
                withAWS(credentials: 'aws-cred', region: 'eu-north-1') {
                sh 'terraform -chdir=eks/ init' 
                // sh 'terraform -chdir=eks/ init -migrate-state'  // Migrate the state to the new backend configuration
                // sh 'terraform -chdir=eks/ init -reconfigure'  // Reconfigure backend without migrating state


                }
            }
        }
        stage('Validate') {
            steps {
                withAWS(credentials: 'aws-cred', region: 'eu-north-1') {
                sh 'terraform -chdir=eks/ validate'
                }
            }
        }
        stage('Action') {
            steps {
                withAWS(credentials: 'aws-cred', region: 'eu-north-1') {
                    script {    
                        if (params.Terraform_Action == 'plan') {
                            sh "terraform -chdir=eks/ plan -var-file=${params.Environment}.tfvars"
                        }   else if (params.Terraform_Action == 'apply') {
                            sh "terraform -chdir=eks/ apply -var-file=${params.Environment}.tfvars -auto-approve"
                        }   else if (params.Terraform_Action == 'destroy') {
                            sh "terraform -chdir=eks/ destroy -var-file=${params.Environment}.tfvars -auto-approve"
                        } else {
                            error "Invalid value for Terraform_Action: ${params.Terraform_Action}"
                        }
                    }
                }
            }
        }
    }
}
