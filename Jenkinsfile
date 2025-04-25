pipeline {
    agent any

    environment {
        ACR_NAME = 'devopsacrbbdc0d'
        IMAGE_NAME = 'flaskapp'
        TAG = 'latest'
        SSH_USER = 'azureuser'
        VM_IP = 'your.vm.ip.address'  // replace with actual IP
        SSH_KEY = credentials('jenkins-ssh-key')  // Jenkins credential ID
    }

    stages {
        stage('Deploy to Azure VM') {
            steps {
                sshagent (credentials: ['jenkins-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $SSH_USER@$VM_IP '
                            docker rm -f flaskapp || true &&
                            az acr login --name $ACR_NAME &&
                            docker pull $ACR_NAME.azurecr.io/$IMAGE_NAME:$TAG &&
                            docker run -d -p 5000:5000 --name flaskapp $ACR_NAME.azurecr.io/$IMAGE_NAME:$TAG
                        '
                    """
                }
            }
        }
    }
}
