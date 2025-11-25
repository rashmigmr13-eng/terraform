pipeline {

    parameters {
        string(name: 'terraformAction', defaultValue: 'apply', description: 'Choose apply or destroy')
    }

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    agent any

    stages {

        stage('Git-Checkout') {
            steps {
                echo "Checking out source code..."
            }
        }

        stage('Plan') {
            steps {
                sh 'pwd; cd project-1/; terraform init'
                sh 'pwd; cd project-1/; terraform plan -out tfplan'
                sh 'pwd; cd project-1/; terraform show -no-color tfplan >> tfplan.txt'
            }
        }

        stage('approval') {
            steps {
                echo "Waiting for approval..."
            }
        }

        stage('Apply or Destroy') {

            when {
                expression {
                    return params.terraformAction == 'apply' || params.terraformAction == 'destroy'
                }
            }

            steps {
                script {

                    if (params.terraformAction == 'apply') {
                        sh 'pwd; cd project-1/; terraform apply -input=false tfplan'
                    } 
                    else if (params.terraformAction == 'destroy') {
                        sh 'pwd; cd project-1/; terraform destroy -auto-approve'
                    }

                }
            }
        }
    }
}

