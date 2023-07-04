pipeline {
    agent any

    environment {
        JENKINS_OPTS = '-Djenkins.model.Jenkins.logStartupPerformance=true -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=300 -Dorg.jenkinsci.plugins.s3.S3ServiceList.BUCKET=jenkinsartifactbucket'
    }

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
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'jenkins-s3-artifact', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        // Upload the tar archive to S3
                        s3Upload(file:'repo_files.tar.gz', bucket:'jenkinsartifactbucket', path:'path/')
                    }
                }
            }
        }
    }
}
