@Library('my-infra-lib') _
pipeline {
    agent {
        label 'Dev'
    }
     parameters {
        choice(name: 'Packer_Build', choices: ['no', 'yes'], description: 'Select an option')
        string(name: 'REGION', defaultValue: 'ap-south-2', description: 'Provide Region')
        choice(name: 'Terraform_Plan', choices: ['no', 'yes'], description: 'Select an option')
        choice(name: 'Terraform_Apply', choices: ['no', 'yes'], description: 'Select an option')
        choice(name: 'Terraform_Destroy', choices: ['no', 'yes'], description: 'Select an option')
        choice(name: 'Pull_AMI', choices: ['no', 'yes'], description: 'Select an option')
    }
    stages {
        stage ('Checking the software'){
            steps {
                sh '''
                terraform version
                packer version
                ansible --version
                aws --version
                '''
            }
        }
        stage ('Packer_Build') {
                when {
                expression { params.Packer_Build == 'yes' }
            }
            steps {
                packerbuild()
            }
        }
        stage('AMI Pull'){
            when{
               expression { params.Pull_AMI == 'yes' } 
            }
            steps{
                fetchami(params.REGION)
            }
        }
        stage ('Terraform_Plan'){
            when {
                expression { params.Terraform_Plan == 'yes' }
            }
            steps{
                terraformplan()
            }
        }
        stage ('Create Infra'){
            when {
                expression { params.Terraform_Apply == 'yes' }
            }
            steps{
                sh 'terraform apply --auto-approve'
            }
        }
        stage ('Destroy Infra'){
            when{
                expression { params.Terraform_Destroy == 'yes' }
            }
            steps{
                sh '''
                terraform init
                terraform destroy --auto-approve
                '''
            }
        }
        stage('Ansible Setup'){
            steps {
                sh 'ansible -i invfile all -m ping'
            }
        }
    }
}