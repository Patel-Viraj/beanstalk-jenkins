pipeline {
    agent any 
    stages {
        stage('Build') {
             steps {
                sh  "echo App is building"
            }
        }
        stage('Test') {
            steps {
                sh  "echo App is Testing"
            }
        }
        stage('make zip') {
            steps {
                sh 'zip -r myzip.zip *'
            }
        }
         stage('Copy to s3') {
            steps {
                sh  "echo Copy to S3"
                sh 'aws s3 cp myzip.zip s3://jenkins-backup-files-sa'
            }
        }
          stage('Create Application') {
            steps {
                sh  "echo Create Application"
                sh 'aws elasticbeanstalk create-application-version --application-name php  --version-label jenkins-pipeline-23  --source-bundle S3Bucket=jenkins-backup-files-sa,S3Key=myzip.zip'
            }
        }
          stage('Update to Beanstalk') {
            steps {
                sh  "echo Update to Beanstalk"
                sh 'aws elasticbeanstalk update-environment --application-name php --environment-name Php-env --version-label jenkins-pipeline-23'
            }
        }
    }
}