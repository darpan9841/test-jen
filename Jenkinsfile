pipeline {
    agent any

    stages {
        stage('Archive Files') {
            steps {
                script {
                    // Create a tar archive of all repository files
                    sh 'tar -czf repo_files.tar.gz .'
                }
            }
        }

        stage('Push to S3') {
            steps {
                script {
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-jenkins-test-demo', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        // Upload the tar archive to S3
                        s3Upload(bucket: 'jenkinsartifactbucket', includePathPattern: 'repo_files.tar.gz', workingDir: '.')
                    }
                }
            }
        }
    }
}