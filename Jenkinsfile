pipeline {
    agent {
        label 'Dev'
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
    }
}