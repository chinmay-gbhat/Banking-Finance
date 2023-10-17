pipeline {
    agent any
    
    environment {
        AWS_ACCESS_ID     = AKIA4VHLQIQ5BYOB7T4O
        AWS_SECRET_KEY    = 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 
  }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/chinmay-gbhat/Banking-Finance.git'
            }
        }
        stage('Build Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('HTML Reports') {
            steps {
               publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Banking-Finance/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
            }
        }
        stage('Build docker image') {
            steps {
                sh 'docker build -t chinmaygbhat/banking-finance:1.0 .'
            }
        }
         stage('Build Push Image') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'Docker_hub', passwordVariable: 'password', usernameVariable: 'docker_login')]) {
                sh 'docker login -u  ${docker_login} -p ${password}'
                }
                sh 'docker push chinmaygbhat/banking-finance:1.0'
            }
        }
        stage ('Configure My Servers with Terraform, Ansible and Deploying '){
            steps {
                dir('MyServer'){
                sh 'sudo chmod 600 AWSec2instancekey.pem'
                sh 'terraform init'
                sh 'terraform validate'
                sh 'terraform apply --auto-approve'
                }
            }
    }
}
}
