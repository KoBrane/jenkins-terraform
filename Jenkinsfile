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
                sh 'terraform plan -out tfplan'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply "tfplan"'
            }
        }
        stage('Terraform Destroy') {
            steps {
                sh 'terraform destroy -auto-approve'
            }
        }       
    }
    //slack notification
    post {
        always {
            echo 'Slack Notifications'
            slacksend channel : '#<your channel name>',
                color: COLOR_MAP[currentBuild.currentResults],
                message: "${currentBuild.currentResult}: Job %{env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }
  }