// Jenkinsfile for deploying a static website on S3 bucket
// pipeline {
//      agent any
//      stages {
//          stage('Build') {
//              steps {
//                  sh 'echo "Hello World"'
//                  sh '''
//                      echo "Multiline shell steps works too"
//                      ls -lah
//                  '''
//              }
//          }      
//          stage('Upload to AWS') {
//               steps {
//                   withAWS(region:'us-east-1',credentials:'AWS Credentials') {
//                   sh 'echo "Uploading content with AWS creds"'
//                       s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'learn-jenkins-deploy-static-website-s3-bucket')
//                   }
//               }
//          }
//      }
// }

//  Jenkinsfile to deploy static website on EC2 instance

pipeline {
  agent any

  environment {
    EC2_USER = "ubuntu"
    EC2_HOST = "3.92.232.209"
    SSH_CRED_ID = "ec2-ssh"
  }

  stages {
    stage('Clone Repo') {
      steps {
        git branch: 'main', url: 'https://github.com/uzairmansoor/learn-jenkins-repo.git'
      }
    }

    stage('Build') {
      steps {
        echo 'Building the app...'
        // sh 'npm install' or 'python setup.py install'
      }
    }

    stage('Deploy to EC2') {
      steps {
        sshagent (credentials: ["${SSH_CRED_ID}"]) {
          sh """
            ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
              echo "Connected to Ubuntu EC2"
              sudo su
              ls -la
              cd /home/ubuntu/
              ls -la
              git clone https://github.com/uzairmansoor/learn-jenkins-repo.git
              cd learn-jenkins-repo
              sudo apt-get update -y
              sudo apt-get install apache2 -y
              sudo cp /home/ubuntu/learn-jenkins-repo/index.html /var/www/html/
            '
          """
        }
      }
    }
  }
  post {
    success {
      echo '✅ Build and deployment succeeded!'
    }
    failure {
      echo '❌ Build and deployment succeeded!'
    }
  }
}
