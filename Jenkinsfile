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
    EC2_HOST = "54.144.17.50"
    SSH_CRED_ID = "ec2-ssh"
  }

  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/uzairmansoor/learn-jenkins-repo.git'
      }
    }

    stage('Deploy to EC2') {
      steps {
        sshagent (credentials: ["${SSH_CRED_ID}"]) {
          sh """
            scp -o StrictHostKeyChecking=no app.zip ${EC2_USER}@${EC2_HOST}:/home/${EC2_USER}/
            ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
              ls -la
              unzip -o app.zip -d app/
              cd app && ./start.sh
            '
          """
        }
      }
    }
  }
}
