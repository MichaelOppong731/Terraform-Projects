pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'eu-west-1'
    }

    parameters {
        booleanParam(name: 'DESTROY_INFRASTRUCTURE', defaultValue: false, description: 'Destroy infrastructure after apply?')
    }

    stages {
        stage('Pull Code') {
            steps {
                git branch: 'main', poll: false, url: 'https://github.com/MichaelOppong731/Terraform-Projects.git'
            }
        }

        stage('Terraform Init & Validate') {
            steps {
                echo "Initializing and validating Terraform"
                
                // Use withCredentials directly in the stage
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_LOGIN']]) {
                    sh '''
                        terraform init
                        terraform fmt -recursive
                        terraform validate
                    '''
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                echo "Generating Terraform plan"
                
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_LOGIN']]) {
                    sh '''
                        terraform plan -out=tfplan
                    '''
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                echo "Applying Terraform changes"
                
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_LOGIN']]) {
                    sh '''
                        terraform apply -auto-approve tfplan
                    '''
                }
            }
        }

        stage('Terraform Destroy') {
            when {
                expression { params.DESTROY_INFRASTRUCTURE }
            }
            steps {
                echo "Destroying Terraform infrastructure"

                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS_LOGIN']]) {
                    sh '''
                        terraform destroy -auto-approve
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Terraform execution complete."
        }
        success {
            echo "Terraform deployment (or destruction) successful."
        }
        failure {
            echo "Terraform deployment (or destruction) failed."
        }
    }
}
