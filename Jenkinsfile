pipeline {
    agent {
        label 'Dev'
    }
     parameters {
        choice(name: 'Packer_Build', choices: ['no', 'yes'], description: 'Select an option')
        choice(name: 'Terraform_Plan', choices: ['no', 'yes'], description: 'Select an option')
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
                sh 'packer plugins install github.com/hashicorp/amazon'
                sh 'packer validate --var-file packer-vars.json packer.json'
            }
        }
        stage ('Terraform_Plan'){
            when {
                expression { params.Terraform_Plan == 'yes' }
            }
            steps{
                sh '''
                terraform init
                terraform fmt
                terraform validate
                terraform plan
                '''
            }
        }
    }
}