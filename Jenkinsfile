pipeline {
    agent any
    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        ECR_REPO = '123456789.dkr.ecr.ap-south-1.amazomaws.com/myapp'
        IMAGE_TAG = 'latest'
    }

    stages { 
        stage('Clone repo') {
            steps { 
                git 'https://github.com/lakshay-11/microservice-app.git'
            }
        }
    
        stage ('Build Docker Image') {
            steps {
                sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
            }
        }

        stage ('Push to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $ECR_REPO
                docker push $ECR_REPO:$IMAGE_TAG
                '''
            }
        }
        
        stage ('Deploy via Ansible') {
            steps {
                sh 'ansible-playbook -i ansible/inventory deploy.yml'
            }
        }
    }
}
