pipeline {
    agent { label 'generic' }
    options {
        buildDiscarder(logRotator(daysToKeepStr: '5', artifactDaysToKeepStr: '5', numToKeepStr: '5'))
        timestamps()
    }

    environment {
        WORKSPACE_DIR = "${env.WORKSPACE}"
        GITHUB_CREDENTIALS_ID = 'git' // Replace with your actual credentials ID
    }

    stages {
        
        stage('Build and Use Image 1') {
            agent {
                dockerfile {
                    label 'generic'
                    filename "first.Dockerfile"
                    additionalBuildArgs '--build-arg WORKSPACE=/workspace'
                    args "-v ${env.WORKSPACE}:/workspace"
                }
            }
            steps {
                sh 'echo "This is some content from container 1" > /workspace/shared_file.txt'
                sh 'echo "File created in container 1"'
            }
        }

        stage('Build and Use Image 2') {
            agent {
                dockerfile {
                    label 'generic'
                    filename "second.Dockerfile"
                    additionalBuildArgs '--build-arg WORKSPACE=/workspace'
                    args "-v ${env.WORKSPACE}:/workspace"
                }
            }
            steps {
                sh 'cat /workspace/shared_file.txt'
                sh 'echo "File contents printed from container 2"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'shared_file.txt',
                             allowEmptyArchive: true,
                             fingerprint: true,
                             onlyIfSuccessful: true
            cleanWs()
        } 
    }
}
