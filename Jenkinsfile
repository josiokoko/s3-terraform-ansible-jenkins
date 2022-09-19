pipeline {
    agent any

    environment {
        PATH = "${PATH}:${getTerraformPath()}"
        PATH = "${PATH}:${getAnsible()}"
    }

    stages{

        stage('s3 Create Bucket'){
            steps{
                sh "ansible-playbook s3-bucket.yml"
            }
        }

        stage('Dev - init and apply'){
            steps{
                sh returnStatus: true, script: 'terraform workspace new dev'
                sh "terraform init"
                sh "ansible-playbook terraform.yml"
            }
        }


        stage('Prod - init and apply'){
            steps{
                sh returnStatus: true, script: 'terraform workspace new prod'
                sh "terraform init"
                sh "ansible-playbook terraform.yml -e app_env=prod"
            }
        }


    }
}


def getTerraformPath(){
    def tfHome = tool name: 'terraform1.2', type: 'terraform'
    return tfHome
}


def getAnsible(){
    def ansibleHome = tool name: 'ansible1.2 ', type: 'org.jenkinsci.plugins.ansible.AnsibleInstallation'
    return ansibleHome
}



