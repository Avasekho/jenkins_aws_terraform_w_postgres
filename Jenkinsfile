/*
Jenkins agent is running on an AWS host;
AWS credentials and ssh-key added to Jenkins credentials;
New instances added to IAM role with access permissions to ECR;
AWS private key file is used in terraform step to wait for ssh up before Ansible step
webapp - https://github.com/deepakrangegowda/Webapp.git
WebPage - localhost:8080/GetStartedTomcat/
*/
pipeline {
  agent {
    label 'jenkins-agent'
  }

  stages {
    stage ('Terraform init') {
      steps {
      sh 'terraform init'
    }
    }

    stage ('Terraform plan') {
      steps {
      sh 'terraform plan'
    }
    }

    stage ('Terraform appy') {
      steps {
        withCredentials([aws(accessKeyVariable:'AWS_ACCESS_KEY_ID', credentialsId: 'aws-credentials', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
        sh 'terraform apply -auto-approve'
    } 
    }
    }

    stage ('Ansible provisioning') {
      steps {
      ansiblePlaybook become: true, credentialsId: 'ansible-ssh', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'provision-playbook.yml'
    }    
    }
  }
}