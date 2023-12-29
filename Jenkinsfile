pipeline {
    agent any

    stages {
        stage('Connect to Azure') {
            steps {
                script {
                    // Azure service principal credentials defined in Jenkins Credentials
                    withCredentials([azureServicePrincipal(credentialsId: 'c9526555-396b-4567-8c14-990e601de11f')]) {
                        // Log in to Azure using Azure CLI
                        bat 'az login --service-principal -u $559223e3-fe8f-4f26-b9f6-e38948d64e55 -p $AuT8Q~-yGonAqZ9hBO2MG1KR4zq.XTWmptcjQac- -t $29c72239-d4e9-4412-baca-f8aa9643f6fb'
                    }
                }
            }
        }
       stage('checkout') {
            steps {
                 script{
                        dir("terraform")
                        {
                            git "https://github.com/chakirimeghana/Terraform-Jenkins.git"
                        }
                    }
                }
            }

        stage('Plan') {
            steps {
                sh 'pwd;cd terraform/ ; terraform init'
                sh "pwd;cd terraform/ ; terraform plan -out tfplan"
                sh 'pwd;cd terraform/ ; terraform show -no-color tfplan > tfplan.txt'
            }
        }
        stage('Approval') {
           when {
      
         not {
                   equals expected: true, actual: params.autoApprove
               }
           }

           steps {
               script {
                    def plan = readFile 'terraform/tfplan.txt'
                    input message: "Do you want to apply the plan?",
                    parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
               }
           }
       }

        stage('Apply') {
            steps {
                sh "pwd;cd terraform/ ; terraform apply -input=false tfplan"
            }
        }
   } 
}
