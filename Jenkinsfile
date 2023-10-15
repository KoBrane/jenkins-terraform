pipeline {
    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

   agent  any
    stages {
        stage('checkout') {
            steps {
                 script{
                        git branch: 'main', url: "https://github.com/KoBrane/jenkins-terraform.git"
                        }
                    }
                }
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Format') {
            steps {
                sh 'terraform fmt'
            }
        }
        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -out tf.plan'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply tfplan -auto-approve'
            }
        }
    }
  }